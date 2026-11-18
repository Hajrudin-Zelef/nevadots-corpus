# Licences, support TAC et RMA Huawei — Guide ultra-complet

> **Public :** chefs de service systèmes & énergies, administrateurs réseau, responsables d'exploitation.
> **Parc de référence :** AP361, AP761 (eKit), S310, AR720, USG6000, eSight, gamme eKit.
> **Langue :** français. Les modèles de tickets TAC sont en anglais (le TAC travaille en anglais).
> **Avertissement :** les procédures, délais et tarifs évoluent. Toute valeur marquée
> **« à vérifier sur le portail officiel »** doit être confirmée sur
> https://support.huawei.com avant engagement contractuel. Aucun identifiant réel
> ne figure dans ce guide : les exemples sont fictifs.
> **Date de rédaction :** septembre 2026.

---

## Table des matières

| Partie | Sections | Contenu |
|---|---|---|
| A — Le système de licences | 1 – 14 | Principes, ESN, types de licences, par produit |
| B — Activer une licence | 15 – 30 | Pas à pas, portail, CLI, erreurs |
| C — Transférer, renouveler, révoquer | 31 – 45 | RMA, renouvellements, registre |
| D — Le portail support.huawei.com | 46 – 60 | Compte, enregistrement, firmwares, HedEx |
| E — Ouvrir un ticket TAC | 61 – 75 | Sévérités, template, suivi, escalade |
| F — Collecter les infos + Garantie | 76 – 90 | diagnostic-information, logs, Hi-Care |
| G — Procédure RMA | 91 – 100 | De la panne au retour du défectueux |
| H — Cas vécus | 101 – 112 | 12 situations réelles commentées |
| I — Checklists et contacts | 113 – 117 | Avant d'acheter, réception, annuel |
| J — Spécificités eKit | 118 – 119 | Le circuit partenaire |
| K — Références | 120 – 123 | Glossaire, quiz, pour aller plus loin |
| L — Annexes opérationnelles | 124 – 143 | Templates, scripts, antisèches, FAQ |

---

# A. LE SYSTÈME DE LICENCES HUAWEI

## 1. Pourquoi la partie administrative compte autant que la technique

Un chef de service peut configurer un USG6000 les yeux fermés ; s'il oublie que la
licence IPS expire un vendredi à 18h00, c'est toute la sécurité périmétrique qui
tombe en mode dégradé pendant le week-end. La partie administrative — licences,
contrats de support, garanties — n'est pas de la paperasse : c'est de la
**disponibilité**. Trois constats de terrain :

- **80 % des « pannes » remontées au TAC un lundi matin sont des licences
  expirées** pendant le week-end (constat empirique des équipes support).
- Un RMA sans contrat Hi-Care, c'est 30 jours ouvrés de délai de réparation
  (Return For Repair) contre 4 heures avec un contrat Premier.
- Un firmware non téléchargeable parce que le contrat logiciel a expiré peut
  bloquer un projet de mise à jour de tout un parc.

Ce guide traite donc les licences, le TAC et le RMA comme des composants
d'exploitation à part entière, avec leurs procédures, leurs SLA et leurs pièges.

## 2. Le triangle : licences, support, garantie

Ces trois notions sont liées mais distinctes. Les confondre est l'erreur n°1.

| Notion | Question à laquelle elle répond | Gérée par | Exemple |
|---|---|---|---|
| **Licence** | « Ai-je le droit d'utiliser cette fonction ? » | Fichier `.dat` lié à l'ESN | Licence IPS sur USG6000 |
| **Support (TAC)** | « Qui m'aide quand ça ne marche pas ? » | Contrat Hi-Care ou garantie | Ticket S2 un dimanche |
| **Garantie** | « Qui remplace le matériel défectueux ? » | Garantie standard ou Hi-Care | RMA d'un S310 HS |

Retenez la formule : **la licence autorise, le support assiste, la garantie
remplace.** Un équipement peut être sous garantie mais sans licence UTM valide :
le TAC vous aidera pour le hardware, mais la fonction IPS restera inactive.

## 3. Vocabulaire de base : ESN, S/N, PN, LAC

| Terme | Signification | Où le trouver |
|---|---|---|
| **ESN** (Equipment Serial Number) | Numéro de série « logique » de l'équipement, utilisé par le système de licences | `display esn`, étiquette, interface web |
| **S/N** (Serial Number) | Numéro de série physique/matériel | Étiquette au dos, `display device` |
| **PN** (Part Number) | Référence commerciale du produit | Bon de commande, étiquette |
| **LAC** (License Authorization Code) | Code d'autorisation de licence (selon les gammes) | Document d'achat de licence |
| **Entitlement ID** | Identifiant de droit d'utilisation, avec mot de passe d'activation | Preuve de droit (Proof of Entitlement) fournie à l'achat |
| **Fichier `.dat`** | Fichier de licence chiffré, lié à un ESN précis | Téléchargé depuis le portail de licences |

> **Point crucial :** l'ESN et le S/N sont souvent identiques ou dérivés l'un de
> l'autre, mais le système de licences ne connaît que l'**ESN**. Toujours copier
> l'ESN affiché par `display esn`, jamais le recopier à la main depuis une
> étiquette si on peut l'éviter.

## 4. L'ESN en détail : l'ancre de tout le système

L'ESN (Equipment Serial Number) est l'identifiant auquel chaque fichier de
licence est cryptographiquement lié. Concrètement :

- Un fichier `.dat` généré pour l'ESN `2102311XYZ...` **ne s'activera jamais**
  sur un équipement d'ESN différent. Le message sera :
  `Error: The ESN of the license file does not match with the device.`
- L'ESN est stable pour la vie de l'équipement, sauf remplacement de la carte
  mère / carte de contrôle (cas RMA : voir section 32).
- Sur les châssis modulaires, il y a un ESN par carte (MPU active/standby) :
  la licence doit correspondre à la carte active.

**Récupérer l'ESN — trois méthodes :**

```
Méthode 1 (recommandée) — en CLI :
<USG6000> display esn
ESN of device: 2102311HSL8H6000123

Méthode 2 — interface web :
System > License Management : l'ESN est affiché en haut de page.

Méthode 3 — étiquette physique :
au dos / dessous de l'équipement, champ « ESN » ou « SN ».
(À n'utiliser qu'en dernier recours : risque d'erreur de recopie.)
```

**Bonnes pratiques ESN :**
- [ ] Toujours copier-coller depuis `display esn`, jamais retaper.
- [ ] Stocker les ESN dans le registre de licences (section 38).
- [ ] Vérifier l'ESN **avant** de générer le fichier sur le portail : le fichier
      est à usage unique, une erreur d'ESN = un nouveau fichier à demander.

## 5. Le fichier de licence `.dat` : anatomie

Le fichier `.dat` est un fichier chiffré généré par Huawei à partir de trois
ingrédients : votre droit d'achat (Entitlement), l'ESN cible, et le contenu de
la licence (fonctions, capacité, durée). Propriétés :

| Propriété | Détail |
|---|---|
| Format | Binaire chiffré, extension `.dat` |
| Liaison | Un fichier = un ESN = un équipement |
| Usage | **Unique** : une fois activé, il est marqué comme consommé côté portail |
| Taille | Quelques kilo-octets |
| Contenu lisible | Aucun (ne pas tenter de l'ouvrir avec un éditeur : il paraîtra corrompu, c'est normal) |
| Transport | TFTP, FTP, SFTP, USB, ou upload via l'interface web |

> **Ne jamais** renommer le fichier avec des caractères spéciaux ou des espaces,
> et ne jamais l'ouvrir/modifier avec un éditeur de texte : le moindre octet
> modifié invalide la signature et provoque `Error: Failed to activate the license`
> ou un diagnostic de « fichier corrompu ».

## 6. Types de licences (1/3) : les licences de fonctionnalités

Elles déverrouillent des fonctions logicielles. Exemples concrets sur le parc
de référence :

| Produit | Licence de fonctionnalité | Effet |
|---|---|---|
| USG6000 | Licence **IPS** (Intrusion Prevention) | Active la détection/prévention d'intrusion |
| USG6000 | Licence **AV** (Antivirus) | Active l'analyse antivirus des flux |
| USG6000 | Licence **URL Filtering** | Active le filtrage d'URL par catégories |
| USG6000 | Licence **Anti-Spam** | Active l'antispam mail |
| USG6000 | Licence **Sandbox / C&C** (selon modèles) | Détection avancée |
| eSight | Licence de **module** (ex. : gestion WLAN, inventaire) | Déverrouille les modules |
| AR720 | Licences de fonctions (ex. : voix, sécurité) | Selon le modèle et la version VRP |

Sans ces licences, l'équipement route et commute normalement, mais les lames
« UTM » (Unified Threat Management) restent inactives. **C'est le cas le plus
fréquent de « panne » un lundi matin** : la licence 1 an achetée avec
l'équipement a expiré pendant le week-end (voir cas vécu n°101).

## 7. Types de licences (2/3) : les licences de capacité

Elles fixent des **quantités** : nombre d'objets gérés, de sessions, d'utilisateurs.

| Produit | Licence de capacité | Exemple |
|---|---|---|
| Contrôleur WLAN / NCE | Nombre d'**AP gérés** | 32, 64, 128 AP... |
| USG6000 | Nombre d'**utilisateurs SSL VPN** concurrents | 50, 100, 200... |
| USG6000 | Nombre de **tunnels IPsec** (selon modèles) | Quotas |
| eSight | Nombre de **nœuds gérés** | Paliers (50/200/1000...) |
| Switch (selon séries) | Ports 40G→100G (licence **RTU** — Right To Use) | Activation de débits |

Dépasser la capacité autorisée se traduit en général par un refus poli :
l'AP n°65 ne montera pas sur le contrôleur, le client VPN n°101 sera rejeté.
Les logs indiquent alors une cause « license » (voir section 76).

## 8. Types de licences (3/3) : temporaires vs permanentes

Huawei distingue (terminologie constatée sur les guides officiels) :

| Type | Durée typique | Usage | Obtenue via |
|---|---|---|---|
| **Commissioning** | 30 jours | Mise en service, tests | Plateforme ESDP |
| **Temporary** | 60 jours | Avant-vente, démo, reprise après incident | Plateforme ESDP |
| **Commercial** | Permanente (ou durée contractuelle) | Production | Achat classique |

Points d'attention :
- Les licences **UTM** (IPS/AV/URL) des USG sont très souvent vendues **à durée
  limitée** (1 an ou 3 ans) : ce sont des abonnements, pas des achats uniques.
- Une licence temporaire expirée **ne se prolonge pas** : il faut en générer
  une nouvelle.
- En cas de RMA, une licence commerciale peut être **transférée** vers le nouvel
  ESN (section 32) ; une licence temporaire, en général, non.

## 9. Licences : cas de l'USG6000

L'USG6000 (HiSecEngine) est l'équipement du parc le plus « gourmand » en
licences. Cartographie :

```
USG6000
├── Licences de base (souvent incluses) : routage, NAT, firewall stateful, IPsec de base
├── Licences UTM (abonnement 1/3 ans, à renouveler !)
│   ├── IPS          → mise à jour des signatures d'attaques
│   ├── AV           → base virale
│   ├── URL Filter   → base de catégories d'URL
│   └── Anti-Spam    → base antispam
├── Licences de capacité
│   ├── Utilisateurs SSL VPN concurrents
│   └── (selon modèle) tunnels / sessions
└── Mises à jour de signatures : nécessitent une licence UTM valide
    → sans licence valide, les signatures ne se mettent plus à jour,
      la protection se dégrade (détection sur des bases obsolètes).
```

**Commandes de contrôle sur USG6000 :**
```
<USG6000> display license
<USG6000> display utm license   (selon version)
```
Vérifiez systématiquement la colonne « Expire Date » (voir section 27).

## 10. Licences : cas d'eSight

eSight (plateforme de supervision) utilise un modèle de licences par **modules**
et par **paliers de nœuds** :

- Chaque grand domaine (réseau, WLAN, inventaire, etc.) peut nécessiter sa
  licence de module.
- Le nombre d'équipements supervisés est plafonné par palier.
- La licence s'installe depuis l'interface d'administration eSight
  (pas en CLI VRP).

> eSight étant en fin de vie commerciale au profit de NCE dans la stratégie
> Huawei récente — **à vérifier sur le portail officiel** pour votre version —,
> vérifiez que vos renouvellements portent bien sur le bon produit.

## 11. Licences : cas des switches S310 et du routeur AR720

- **S310** : switch d'accès eKit/entreprise. La plupart des fonctions L2/L3 de
  base ne demandent pas de licence. Les fonctions avancées (selon modèle :
  stacking iStack, routage avancé) peuvent nécessiter des licences RTU.
- **AR720** : routeur de la gamme NetEngine AR. Licence de base incluse ;
  fonctions voix, sécurité avancée ou capacité (tunnels) sous licence selon
  la référence commandée.

**Réflexe :** à l'achat, exiger du partenaire la liste écrite des licences
**incluses** vs **optionnelles** avec leur durée (voir checklist section 113).

## 12. Licences : cas des AP361 / AP761 (Wi-Fi)

Bonne nouvelle : sur les AP Huawei (dont AP361 et AP761 eKit), il n'y a en
général **pas de licence par AP** à acheter. Le modèle est :

- Les AP sont gérés par un contrôleur (WAC), par NCE, ou en mode **cloud/local**
  selon la gamme.
- C'est le **contrôleur** qui porte la licence de capacité « nombre d'AP gérés ».
- En mode eKit (gestion via l'application / le cloud eKit), la gestion est
  simplifiée et ne demande pas de licence AP unitaire — **à vérifier sur le
  portail officiel** pour votre mode de gestion exact.

Conséquence pratique : si un AP ne « monte » pas sur le contrôleur alors que
les autres fonctionnent, pensez « capacité de licence du contrôleur atteinte »
avant « AP défectueux ».

## 13. Licences : NCE et autres plateformes

NCE (Network Cloud Engine), successeur logique d'eSight/ Agile Controller dans
la stratégie Huawei, utilise des licences par contrôleur et par ressources
gérées. Les principes restent identiques (ESN, `.dat`, portail), seule
l'interface d'import change. **À vérifier sur le portail officiel** pour la
version exacte que vous déployez.

## 14. Où sont stockées les licences sur l'équipement

- Le fichier `.dat` est copié dans la **mémoire flash** de l'équipement
  (répertoire visible via `dir`).
- L'activation (`license active`) enregistre la licence dans la zone dédiée ;
  elle **survit au reboot** (pas besoin de réactiver après redémarrage).
- Elle **ne survit pas** à un remplacement de carte mère / MPU (nouvel ESN) :
  prévoir le transfert (section 32).
- En cluster actif/standby (ex. : deux USG en HA), **chaque membre** doit avoir
  sa propre licence activée avec son propre ESN.

**Checklist de fin de partie A :**
- [ ] Je sais distinguer licence / support / garantie.
- [ ] Je sais récupérer un ESN par `display esn`.
- [ ] Je connais les licences à durée limitée de mon parc (UTM USG6000 en tête).
- [ ] J'ai identifié qui porte les licences de capacité (contrôleur WLAN...).

---

# B. ACTIVER UNE LICENCE PAS À PAS

## 15. Vue d'ensemble du workflow

```
┌─────────────┐    ┌──────────────┐    ┌───────────────┐    ┌──────────────┐    ┌────────────┐
│ 1. Récupérer │───▶│ 2. Preuve de │───▶│ 3. Générer le │───▶│ 4. Transférer │───▶│ 5. Activer │
│    l'ESN     │    │ droit (EID + │    │ fichier .dat  │    │ le .dat sur   │    │ license    │
│ display esn  │    │  mot de passe)│    │ sur le portail│    │ l'équipement  │    │ active     │
└─────────────┘    └──────────────┘    └───────────────┘    └──────────────┘    └────────────┘
                                                                                       │
                                                                              ┌────────▼────────┐
                                                                              │ 6. Vérifier     │
                                                                              │ display license │
                                                                              └─────────────────┘
```

Durée typique : 15 à 30 minutes quand on a tous les éléments ; 2 heures à 2
jours quand il manque la preuve de droit (voir cas vécu n°104).

## 16. Étape 1 — Récupérer l'ESN

Voir section 4. En pratique, sur un équipement en production :

```
<USG6000> display esn
ESN of device: 2102311HSL8H6000123
```

- Copier la valeur **exacte** (copier-coller, pas de recopie manuelle).
- La coller immédiatement dans le registre de licences (section 38) et dans
  le formulaire du portail.
- Sur un châssis à double MPU : relever l'ESN des **deux** cartes
  (`display esn slot X` selon les modèles — **à vérifier sur le portail
  officiel** pour votre châssis).

## 17. Lecture commentée d'un `display esn`

```
<USG6000> display esn
ESN of device: 2102311HSL8H6000123
│               └───────────────────── 19 caractères alphanumériques
│                 • les 7 premiers identifient en général la série/le site de fabrication
│                 • la suite est le numéro unique
└─ Un seul ESN affiché = équipement monobloc (pas de châssis multi-cartes)
```

Si la commande retourne une erreur ou un champ vide : l'équipement n'a pas
d'ESN programmé (rare, matériel très ancien ou carte remplacée sans
reprogrammation) → ouvrir un ticket TAC, seul Huawei peut régénérer un ESN.

## 18. Étape 2 — La preuve de droit (Entitlement ID + mot de passe)

À l'achat d'une licence, le partenaire (ou Huawei) fournit un document
« Proof of Entitlement » (preuve de droit) contenant :

| Champ | Exemple fictif | Usage |
|---|---|---|
| Entitlement ID | `EID-2026-8F3K2P` *(fictif)* | Identifie votre droit sur le portail |
| Activation Password | `A1b2-C3d4-E5f6` *(fictif)* | Mot de passe à saisir pour générer le fichier |
| Part Number | `L-USG-IPS-1Y` *(fictif)* | Référence de la licence achetée |
| Quantité / durée | 1 / 1 an | Ce que vous avez payé |

**Où trouver ce document ?**
1. Dans les pièces jointes du bon de livraison / de la facture du partenaire.
2. Dans l'espace client du partenaire (portail revendeur).
3. En le redemandant au commercial du partenaire (prévoir 24-48h).

> **Piège classique :** le commercial envoie la facture mais pas la preuve de
> droit. Sans Entitlement ID + mot de passe, impossible de générer le `.dat`.
> Exigez ce document **à la commande** (checklist section 113).

## 19. Étape 3 — Le portail de téléchargement des licences

Le portail officiel de gestion des licences électroniques est accessible via
le support Huawei (adresse constatée : `https://app.huawei.com/isdp` — **à
vérifier sur le portail officiel**, l'URL peut évoluer ; en cas de doute,
partez de https://support.huawei.com > rubrique Licences).

Prérequis :
- Un compte support.huawei.com (voir section 47).
- L'Entitlement ID et le mot de passe d'activation.
- L'ESN exact de l'équipement cible.

## 20. Génération unitaire : le formulaire ESN

Procédure type (les intitulés exacts peuvent varier) :

1. Connectez-vous au portail de licences.
2. Saisissez l'**Activation Password** de votre preuve de droit, cliquez sur
   **Activate**.
3. Le portail affiche les informations du droit (part number, quantité, durée,
   fonctions). **Vérifiez-les** : c'est votre dernière chance de détecter une
   erreur de commande (mauvais produit, mauvaise durée).
4. Saisissez l'**ESN** (copié depuis `display esn`).
5. Validez : le portail génère le fichier `.dat`.
6. Téléchargez-le immédiatement et archivez-le (voir section 22).

> **Rappel :** le fichier est à usage unique et lié à l'ESN saisi. Une faute
> de frappe sur l'ESN = fichier inutilisable = nouvelle demande à formuler.

## 21. Génération en masse : le Batch Import

Pour un parc (ex. : 40 AP gérés = licences contrôleur, ou plusieurs USG) :

1. Sur le portail, téléchargez le **template** (modèle de fichier) proposé
   pour l'import en masse.
2. Remplissez une ligne par équipement : **Password** (mot de passe
   d'activation) + **ESN**.
3. Cliquez sur **Batch Import** et téléversez le fichier rempli.
4. Le portail génère les `.dat` en lot ; téléchargez l'archive.

**Conseils :**
- Préparez le fichier depuis votre registre de licences (section 38) : une
  ligne = un équipement, zéro recopie manuelle.
- Vérifiez le rapport d'import ligne par ligne avant de quitter la page.

## 22. Télécharger le `.dat` : les vérifications immédiates

Dès le téléchargement :

- [ ] Le nom du fichier mentionne-t-il le produit / l'ESN ? (selon le portail)
- [ ] La taille est-elle plausible (quelques Ko, pas 0 octet) ?
- [ ] Archivez-le dans `licences/<année>/<équipement>_<ESN>_<fonction>.dat`
      **et** dans la sauvegarde hors site.
- [ ] Notez dans le registre : date de génération, date d'expiration,
      Entitlement ID d'origine.

Ne supprimez jamais l'original téléchargé : en cas de RMA, il servira de
preuve pour demander le transfert (section 32).

## 23. Étape 4 — Transférer le fichier sur l'équipement

Quatre méthodes, de la plus simple à la plus « terrain » :

| Méthode | Commande / action | Quand l'utiliser |
|---|---|---|
| **Web** | System > License Management > Upload | Accès HTTPS disponible, petit fichier |
| **TFTP** | `tftp <serveur> get licence.dat` | Réseau local, serveur TFTP dispo |
| **FTP/SFTP** | `ftp` / `sftp` puis `get` | Transfert authentifié |
| **USB** | Copier sur clé, `copy usb:/licence.dat flash:/` | Site isolé, pas de réseau |

Vérifiez la présence après transfert :
```
<USG6000> dir
...
licence.dat   (taille cohérente avec l'original)
```

## 24. Étape 5 — Activer : `license active` (CLI)

```
<USG6000> system-view
[USG6000] license active licence.dat
Info: License file activated successfully.
```

Points d'attention :
- Le nom du fichier doit être **exact** (sensible à la casse selon les versions).
- Ne lancez **jamais** l'activation pendant un basculement actif/standby :
  en cas de switchover durant l'activation, le fichier peut échouer à se
  « dispatcher » (erreur documentée : *Failed to dispatch the license file*).
  Si cela arrive : attendre la fin du basculement, puis réactiver.
- Sur les châssis double MPU : vérifier l'espace disque de la MPU standby
  avant d'activer (erreur documentée : disque plein côté standby).

## 25. Étape 5 bis — Activer via l'interface web

1. Connectez-vous à l'interface web d'administration.
2. Allez dans **System > License Management**.
3. Cliquez **Upload / Activate**, sélectionnez le `.dat`.
4. Validez : le système affiche le résultat et le récapitulatif de la licence.
5. **Rechargez la page** et vérifiez que la licence apparaît avec le bon
   statut et la bonne date d'expiration.

L'activation web et l'activation CLI sont équivalentes ; choisissez selon
vos habitudes et vos droits d'accès.

## 26. Étape 6 — Vérifier : `display license`

```
<USG6000> display license
MainBoard:
  ESN code          : 2102311HSL8H6000123
  License state     : Demo / Normal / Expired
  License file name : licence.dat
  ...
  Feature           : IPS
  Expire date       : 2027-09-26
```

Ce que vous devez voir : **le bon ESN**, le bon fichier, chaque fonction
attendue avec sa date d'expiration. Tout écart = retour à l'étape concernée.

## 27. Lecture commentée d'un `display license`

```
<USG6000> display license
MainBoard:
  ESN code          : 2102311HSL8H6000123   ← doit matcher display esn
  License state     : Normal                ← « Normal » = OK
                                                « Demo » = licence d'essai/temporaire
                                                « Expired » = expirée → renouveler
  License file name : licence.dat           ← le fichier activé
  Created time      : 2026-09-27            ← date d'activation
  Feature           : IPS                   ← une ligne par fonction
  Expire date       : 2027-09-26            ← SURVEILLER : mettre une alerte J-90
  Feature           : AV
  Expire date       : 2027-09-26
  Feature           : URL Filtering
  Expire date       : 2027-09-26
```

**Automatisation conseillée :** un script qui se connecte en SSH chaque nuit,
lance `display license` sur les USG, parse les dates d'expiration et alerte
à J-90 / J-60 / J-30. C'est le garde-fou qui évite le cas vécu n°101.

## 28. Cas particulier USG6000 : activer la licence UTM complète

Sur les USG récents (HiSecEngine), la procédure suit le même schéma, avec
un guide dédié (« License Usage Guide » du modèle). Particularités :

- Les licences UTM sont gérées comme un **lot** (IPS+AV+URL) mais peuvent
  avoir des dates distinctes si achetées séparément.
- Après activation, forcez une **mise à jour des signatures** pour valider
  que la licence est bien prise en compte :
  `update signature` (commande exacte **à vérifier sur le portail officiel**
  pour votre version VRP).
- En HA actif/standby : activez sur les **deux** membres, chacun avec son
  propre fichier (deux ESN = deux fichiers).

## 29. Cas particulier eSight : charger la licence

1. Connectez-vous à la console d'administration eSight.
2. Rubrique **System > License Management** (intitulé exact selon version).
3. Importez le fichier de licence (format spécifique eSight, pas forcément
   `.dat` — **à vérifier sur le portail officiel**).
4. Vérifiez les modules déverrouillés et le compteur de nœuds.
5. Redémarrez les services concernés si la documentation l'exige.

## 30. Erreurs classiques d'activation : tableau de dépannage

Tableau établi d'après la documentation officielle de dépannage des licences
(HiSecEngine USG6000F License Usage Guide) :

| Message affiché | Cause probable | Conduite à tenir |
|---|---|---|
| `The ESN of the license file does not match with the device` | ESN saisi erroné sur le portail | 1. Redémarrer l'équipement et revérifier l'ESN (`display esn`). 2. Si l'ESN du fichier reste différent : **régénérer un fichier avec le bon ESN**. |
| `The product type of the license file does not match with the device` | Fichier généré pour un autre modèle | Régénérer un fichier pour le bon type de produit. |
| `The license file has expired` | Fichier généré depuis trop longtemps ou licence à durée dépassée | Obtenir un fichier non expiré / renouveler la licence. |
| `Failed to dispatch the license file` | Échec de transmission : disque standby plein, fichier absent, basculement actif/standby pendant l'activation | Libérer de l'espace sur la MPU standby ; vérifier la présence du fichier ; ne pas basculer pendant l'activation, puis réactiver. |
| `The license file has been activated` | Fichier déjà activé | Rien à faire : c'est normal, pas une erreur. |
| `Failed to activate the license` | Fichier corrompu ou problème interne | Vérifier l'intégrité (re-télécharger, re-transférer) ; si persistant : contacter le support technique. |

**Règle d'or :** devant une erreur, toujours vérifier dans l'ordre :
1) l'ESN (`display esn`), 2) le type de produit, 3) l'intégrité du fichier,
4) l'espace disque, 5) l'absence de basculement HA en cours.

---

# C. TRANSFÉRER, RENOUVELER, RÉVOQUER UNE LICENCE

## 31. Principe fondamental : la licence suit l'ESN, pas le propriétaire

Une licence Huawei est liée à **un ESN précis**. Elle ne se « déplace » pas
toute seule :

- Changer d'équipement = nouvel ESN = **nouveau fichier** à générer.
- Il n'existe pas de « copier-coller » du `.dat` d'un équipement vers un autre :
  l'activation échouera (ESN mismatch, section 30).
- Le transfert est une **procédure administrative** : on demande à Huawei (via
  le partenaire ou le portail) de réémettre la licence pour le nouvel ESN,
  en justifiant (RMA, remplacement, erreur d'ESN initiale).

## 32. Cas du RMA : transférer la licence vers l'équipement de remplacement

C'est le scénario le plus courant. Procédure type :

```
1. Le TAC valide le RMA et vous expédie l'équipement de remplacement.
2. À réception, relevez le NOUVEL ESN : display esn  →  ESN_NEW
3. Rassemblez : ancien ESN (ESN_OLD), numéro de RMA, preuve d'achat initiale.
4. Demandez le transfert :
   a) via votre partenaire (canal recommandé : il connaît votre dossier), ou
   b) via le portail de licences (fonction de réémission / rehost — intitulé
      exact à vérifier sur le portail officiel).
5. Générez le nouveau .dat pour ESN_NEW, activez-le (partie B).
6. Vérifiez : display license sur le nouvel équipement.
7. Mettez à jour le registre de licences (section 38) : l'ancien ESN passe
   au statut « retourné / révoqué ».
```

**Délais :** le transfert prend en général 24 à 72h ouvrées via le partenaire.
**Ne renvoyez pas l'ancien équipement avant d'avoir le nouveau fichier** si
vous devez encore l'utiliser (période de chevauchement à négocier).

## 33. Renouveler une licence UTM (le workflow annuel)

Les licences IPS/AV/URL des USG6000 sont des abonnements. Le renouvellement
annuel suit ce cycle :

```
J-90  : alerte automatique (script display license, section 27)
        → informer le N+1 / les achats du budget à prévoir
J-60  : demander un devis au partenaire (licence 1 an ou 3 ans)
J-30  : passer commande ; exiger la Proof of Entitlement
J-15  : générer le .dat (nouvel ESN ? non : même équipement, même ESN)
J-7   : activer le nouveau fichier (l'ancien reste valide jusqu'à sa fin)
J     : vérifier display license → nouvelle date d'expiration
```

**3 ans vs 1 an :** le 3 ans coûte moins cher par an en général (**tarifs à
vérifier auprès du partenaire**) et divise par trois la charge administrative.
Recommandé pour les équipements critiques.

## 34. Le calendrier de renouvellement : l'outil du chef de service

Tenez un calendrier partagé (ou un simple tableur) avec, pour chaque licence
à durée limitée :

| Équipement | Fonction | Expiration | Alerte J-90 | Alerte J-60 | Statut | Responsable |
|---|---|---|---|---|---|---|
| USG6000-PARIS | IPS/AV/URL | 2027-09-26 | 2027-06-28 | 2027-07-28 | À commander | Zelef |
| USG6000-LYON | IPS/AV/URL | 2027-11-15 | 2027-08-17 | 2027-09-16 | Devis demandé | [À COMPLÉTER] |

Revue **mensuelle** en réunion d'équipe : 5 minutes suffisent à éviter 90 %
des expirations surprises.

## 35. Révoquer / désactiver une licence

Cas d'usage : équipement mis au rebut, licence transférée, erreur d'activation.
Méthodes :

- **Côté équipement :** selon les modèles, `license revoke` / `undo license
  active` ou suppression via l'interface web (commande exacte **à vérifier
  sur le portail officiel** pour votre version).
- **Côté portail :** marquer le droit comme révoqué/retourné pour libérer
  l'Entitlement si le contrat le permet.

> Une licence révoquée côté équipement mais pas côté portail peut bloquer une
> réémission ultérieure : faites les deux.

## 36. Licence expirée : ce qui se passe techniquement

Comportement typique (varie selon produit et version) :

| Produit | À l'expiration |
|---|---|
| USG6000 (UTM) | Les lames IPS/AV/URL cessent de se mettre à jour puis se désactivent ; le firewall de base continue. Alarmes/logs « license expired ». |
| Contrôleur WLAN | Les AP en dépassement de capacité ne montent plus ; les AP existants peuvent rester (selon version) ou tomber. |
| eSight | Modules sous licence désactivés ; supervision réduite. |

**Ce n'est jamais silencieux** : des alarmes sont générées. Le problème, c'est
qu'elles arrivent souvent un vendredi soir et que personne ne les regarde
avant lundi (cas vécu n°101).

## 37. Période de grâce : mythe ou réalité ?

Certains produits appliquent une courte période de grâce après expiration
(quelques jours) pendant laquelle les fonctions restent actives avec des
avertissements. **Ce comportement varie selon les produits et les versions —
à vérifier sur le portail officiel** pour chacun de vos équipements.

**Règle de pilotage :** ne jamais compter sur une période de grâce. Pilotez
au J-90 comme si l'expiration était couperet.

## 38. L'inventaire des licences : construire le registre

Le registre de licences est **le** document administratif du chef de service.
Une ligne par licence, les colonnes suivantes au minimum :

| Colonne | Exemple fictif |
|---|---|
| Équipement (nom) | USG6000-Paris |
| Modèle | USG6680 *(fictif)* |
| ESN | 2102311HSL8H6000123 *(fictif)* |
| Fonction / capacité | IPS+AV+URL Filtering |
| Part Number | L-USG-IPS-1Y *(fictif)* |
| Entitlement ID | EID-2026-8F3K2P *(fictif)* |
| Date d'achat | 2026-09-26 |
| Date d'activation | 2026-09-27 |
| Date d'expiration | 2027-09-26 |
| Type | Commercial 1 an |
| Fichier .dat | `licences/2026/USG6000-Paris_21023..._IPS.dat` |
| Fournisseur | [À COMPLÉTER] |
| Statut | Active / Expirée / Transférée / Révoquée |
| Notes | Renouvellement 3 ans envisagé |

Stockage : tableur partagé + sauvegarde. Accès : chef de service + adjoint
a minima (pas sur le seul PC d'une personne).

## 39. Template de registre prêt à copier (CSV)

```csv
equipement;modele;esn;fonction;part_number;entitlement_id;date_achat;date_activation;date_expiration;type;fichier_dat;fournisseur;statut;notes
USG6000-Paris;USG6680;2102311HSL8H6000123;IPS+AV+URL;L-USG-IPS-1Y;EID-2026-8F3K2P;2026-09-26;2026-09-27;2027-09-26;Commercial 1 an;licences/2026/USG6000-Paris_IPS.dat;Partenaire X;Active;Passer en 3 ans en 2027
S310-BatA;S310-24P;2102311ABC...;Base;Incluse;-;-;-;-;Permanent;-;Partenaire X;Active;Rien à renouveler
```

*(ESN et références ci-dessus sont fictifs : remplacez par vos valeurs.)*

## 40. L'audit annuel des licences

Une fois par an (idéalement 3 mois avant la clôture budgétaire) :

1. [ ] Exporter le registre complet.
2. [ ] Pour chaque équipement critique : `display license` réel vs registre
      (écarts = licences activées sans trace, ou expirations non vues).
3. [ ] Lister les expirations des 12 prochains mois + budget associé.
4. [ ] Identifier les licences inutilisées (fonction payée jamais activée).
5. [ ] Vérifier les transferts en cours (RMA de l'année).
6. [ ] Présenter la synthèse à la direction : budget N+1.

## 41. Licences et haute disponibilité (actif/standby)

- Chaque membre d'un cluster HA a son **propre ESN** et donc besoin de son
  **propre fichier** `.dat`.
- Les deux fichiers doivent couvrir les **mêmes fonctions** avec des dates
  cohérentes, sinon le basculement dégrade le service (IPS actif d'un côté,
  inactif de l'autre).
- À l'achat d'un couple HA : commander **2×** chaque licence.

## 42. Licences et remplacement de carte (châssis modulaires)

Sur châssis (S12700, etc. — pas votre parc actuel, mais à savoir) :
- La licence est liée à l'ESN de la **MPU active**.
- Remplacement de la MPU = nouvel ESN = transfert de licence (section 32).
- Prévoir dans le contrat de maintenance une clause de **transfert express**
  en cas de remplacement de carte.

## 43. Coûts : ordres de grandeur et vigilance

Les tarifs des licences varient selon les gammes, les durées et les remises
partenaires. **Aucun prix n'est donné ici : tout tarif est à vérifier auprès
de votre partenaire.** En revanche, retenez les règles de gestion :

- Budgétez les renouvellements UTM comme des **charges récurrentes**
  (OPEX), pas comme des investissements ponctuels.
- Un devis doit toujours détailler : part number, quantité, durée, dates
  de début/fin de droits.
- Comparez le 1 an vs 3 ans **à service égal** (mêmes fonctions, mêmes dates).

## 44. Négocier les renouvellements avec le partenaire

Leviers d'un chef de service :

1. **Anticipation** (J-90) : commander dans l'urgence coûte toujours plus cher.
2. **Volume** : regrouper les renouvellements de plusieurs sites sur un seul
   bon de commande.
3. **Durée** : le 3 ans fait baisser le coût annuel et la charge de gestion.
4. **Alignement des dates** : faire coïncider les expirations (co-terming)
   pour une seule négociation annuelle.
5. **Concurrence loyale** : faire chiffrer par deux partenaires agréés.

## 45. Pièges classiques du cycle de vie des licences

| Piège | Conséquence | Parade |
|---|---|---|
| Preuve de droit jamais réclamée | Impossible de générer le `.dat` | L'exiger à la commande (checklist 113) |
| ESN recopié à la main | Fichier inutilisable | Copier-coller depuis `display esn` |
| Renouvellement commandé trop tard | Expiration + réactivation en urgence | Alertes J-90/J-60/J-30 |
| Licence activée sur un seul membre du HA | Dégradation au basculement | 2 fichiers, 2 ESN, vérification croisée |
| `.dat` perdu (pas d'archive) | Transfert RMA compliqué | Archivage systématique (section 22) |
| Registre non tenu | Aucune visibilité | Audit annuel (section 40) |
| Licence temporaire oubliée | Expiration à 60 jours en pleine prod | Ne jamais mettre de temporaire en production sans plan de bascule |

---

# D. LE PORTAIL SUPPORT HUAWEI (support.huawei.com)

## 46. Présentation : la tour de contrôle de votre parc

https://support.huawei.com (rubrique Enterprise) est le point d'entrée unique
pour :

- Enregistrer vos équipements (par numéro de série).
- Télécharger **firmwares, patchs et documentation**.
- Gérer les **licences** (lien avec les parties B et C).
- Vérifier les **garanties**.
- Ouvrir et suivre les **tickets TAC**.
- Accéder à la **base de connaissances** et à la communauté.

> Sans compte, l'accès est limité (documentation publique partielle). Avec un
> compte et des équipements enregistrés, les téléchargements logiciels sont
> débloqués **selon vos droits contractuels** (voir cas vécu n°105).

## 47. Créer un compte, pas à pas

1. Allez sur https://support.huawei.com → **Register / S'enregistrer**.
2. Renseignez : nom, prénom, **email professionnel** (évitez les adresses
   personnelles : le compte suit le collaborateur, pas l'entreprise —
   voir section 60), entreprise, pays, téléphone.
3. Choisissez un mot de passe robuste (gestionnaire de mots de passe
   recommandé) et activez la double authentification si proposée.
4. Validez l'email de confirmation.
5. Complétez le profil : rôle (administrateur réseau, chef de service...),
   secteur d'activité.

**Délai :** le compte est actif en quelques minutes ; certaines fonctions
(téléchargements logiciels) peuvent nécessiter une validation supplémentaire
liée à vos contrats.

## 48. Les rôles et la gestion multi-utilisateurs

Pour une équipe (chef de service + techniciens) :

- Créez **un compte par personne** (traçabilité des tickets et des
  téléchargements).
- Désignez un **administrateur du compte entreprise** qui gère les accès.
- À chaque départ d'un collaborateur : révoquer son accès et transférer ses
  tickets ouverts (ne jamais partager un compte unique : en cas d'audit,
  impossible de savoir qui a fait quoi).

## 49. Enregistrer ses équipements (par numéro de série)

C'est l'étape qui débloque les droits (téléchargements, garantie, TAC) :

1. Dans votre espace : **My Devices / Mes équipements** → **Register**.
2. Saisissez le **numéro de série** (S/N) de l'équipement — celui de
   l'étiquette physique ou de `display device` / `display esn`.
3. Ajoutez : modèle, site d'installation, date de mise en service.
4. Répétez pour **tout le parc** (AP361, AP761, S310, AR720, USG6000...).

**Astuce :** faites-le **à la réception** du matériel (checklist section 114),
pas six mois plus tard quand vous aurez besoin d'un firmware en urgence.

## 50. Le tableau de bord : la vue d'ensemble du parc

Une fois les équipements enregistrés, le portail affiche typiquement :

| Vue | Utilité pour le chef de service |
|---|---|
| Liste des équipements | Inventaire officiel, exportable |
| Statut de garantie par S/N | Repérer les fins de garantie (revue annuelle) |
| Contrats associés (Hi-Care) | Vérifier la couverture de chaque site |
| Alertes (bulletins de sécurité) | Anticiper les patchs critiques |
| Tickets ouverts | Suivi sans appeler la hotline |

## 51. Télécharger firmwares et patchs

1. Recherchez votre modèle (ex. : `USG6680`, `S310-24P`).
2. Onglet **Software** : versions VRP, patchs, notes de version.
3. **Lisez toujours la Release Note avant de télécharger** : prérequis,
   incompatibilités, procédure de mise à jour.
4. Téléchargez via une connexion stable ; vérifiez la somme de contrôle
   (hash) si fournie.

> **Point de vigilance :** le téléchargement de certaines versions logicielles
> exige un contrat de support logiciel actif. Sans contrat : accès refusé
> (cas vécu n°105). C'est contractuel, pas un bug du site.

## 52. Choisir la bonne version : matrice de compatibilité

Avant toute montée de version :

- [ ] Consulter la **matrice de compatibilité** (version VRP ↔ modèle ↔
      fonctionnalités) sur le portail.
- [ ] Vérifier que vos licences restent valides sur la version cible
      (rarement un problème, mais à contrôler).
- [ ] Lire les « upgrade paths » : certaines versions exigent un palier
      intermédiaire (ex. : V500 → V600 puis V700, pas de saut direct).
- [ ] Tester sur un équipement non critique ou en maquette.

## 53. Télécharger la documentation

Le portail donne accès aux guides d'installation, de configuration, de
maintenance et aux références de commandes par modèle et par version.
**Réflexe :** toujours télécharger la doc de **votre version exacte**
(la syntaxe CLI change entre versions VRP).

## 54. HedEx : présentation

**HedEx** (Huawei Electronic Documentation Explorer) est le lecteur de
documentation électronique de Huawei :

- Format propriétaire **.hdx** : documentation structurée, consultable
  hors ligne après téléchargement.
- Alternative/complément au PDF : recherche plein texte, navigation par
  arborescence, index.
- Le lecteur HedEx se télécharge depuis le portail support.

Usage typique : embarquer toute la doc d'une gamme sur le PC portable du
technicien qui part sur un site isolé sans internet.

## 55. HedEx : recherche et navigation efficaces

- Utilisez la **recherche plein texte** avec des mots-clés CLI exacts
  (`display license`, `license active`) plutôt que des phrases.
- Naviguez par **arborescence produit → version → guide** pour éviter les
  docs d'une autre version.
- Mettez en **favoris** les pages utilisées en intervention (procédures
  d'urgence, codes d'erreur).

## 56. Le centre de licences (lien avec les parties B–C)

Depuis le portail support, la rubrique **Licenses** (ou via le portail
dédié, adresse constatée `https://app.huawei.com/isdp` — **à vérifier sur
le portail officiel**) permet de :

- Générer les fichiers `.dat` (sections 20–21).
- Consulter l'historique des générations (quel ESN, quand, quel droit).
- Suivre les consommations d'Entitlements (droits restants sur un lot).
- Initier des transferts / réémissions (section 32).

## 57. Vérifier la garantie d'un équipement par S/N

1. Équipements enregistrés (section 49) → fiche de l'équipement.
2. La fiche affiche : **date de début / fin de garantie**, niveau de service
   (standard ou Hi-Care avec son palier).
3. En cas d'écart avec votre contrat : capture d'écran + ticket au
   partenaire (c'est lui qui fait corriger les données).

**À faire une fois par an** pour tout le parc (checklist section 115).

## 58. La communauté et la base de connaissances

Avant d'ouvrir un ticket TAC (partie E) :

1. Recherchez le message d'erreur exact dans la base de connaissances.
2. Consultez les **cas de maintenance** publiés (souvent des pannes
   identiques déjà résolues).
3. Posez la question sur la **communauté technique** Huawei pour les
   sujets non urgents (S3/S4).

Un ticket ouvert avec « j'ai vérifié la base de connaissances, cas
n°XXX, sans succès » est traité plus vite : vous avez fait la moitié du
diagnostic.

## 59. L'application mobile et les autres canaux

Huawei propose des canaux mobiles (application de support, chatbot
intelligent — noms exacts **à vérifier sur le portail officiel**) permettant
de :

- Suivre les tickets en déplacement.
- Recevoir les notifications de bulletins de sécurité.
- Accéder à la documentation.

Utile pour le chef de service d'astreinte : le suivi d'un ticket S1/S2
depuis le téléphone, un dimanche soir.

## 60. Bonnes pratiques du portail (à afficher dans le bureau)

- [ ] Un compte **par personne**, email professionnel, 2FA activée.
- [ ] Équipements enregistrés **à la réception** (pas « quand on aura le temps »).
- [ ] Droits d'admin du compte entreprise : **2 personnes minimum**
      (chef + adjoint) pour éviter le blocage en cas d'absence.
- [ ] Export trimestriel de la liste des équipements + garanties.
- [ ] Ne jamais partager les identifiants ; révoquer les accès des partants.
- [ ] En cas de changement de partenaire : vérifier que les enregistrements
      et les droits suivent (nouveau partenaire = nouvelles preuves de droit).

---

# E. OUVRIR UN TICKET TAC

## 61. Le TAC : qu'est-ce que c'est, comment y accéder

Le **TAC** (Technical Assistance Center) est le centre d'assistance technique
de Huawei. Il fournit de l'assistance à distance (diagnostic, contournement,
résolution) **24h/24, 7j/7** pour les clients couverts (garantie ou contrat
Hi-Care — voir partie F).

Canaux d'accès constatés (d'après la documentation Hi-Care) :
- **Hotlines TAC** (numéros par région — voir section 117).
- **Email** (adresses régionales, ex. : support Europe).
- **Portail web** : ouverture et suivi des « Service Requests ».
- **Application mobile** de support.

> Le délai de réponse contractuel court **à partir de l'acceptation de votre
> demande par le TAC** jusqu'au premier contact d'un ingénieur — pas à partir
> de l'envoi de votre email. D'où l'intérêt du portail (horodatage fiable).

## 62. Avant d'appeler : la checklist pré-ticket

Un ticket bien préparé se résout 2 à 3 fois plus vite. Avant tout contact :

- [ ] Le problème est-il reproductible ? (oui / non / intermittent)
- [ ] Avez-vous le modèle exact, la version VRP, l'ESN/S/N ? (section 69)
- [ ] Avez-vous collecté `display diagnostic-information` ? (section 76)
- [ ] Avez-vous cherché dans la base de connaissances ? (section 58)
- [ ] Quel est l'impact métier ? (qui est touché, depuis quand)
- [ ] Avez-vous un contournement temporaire en place ?
- [ ] Qui est joignable 24h/24 si le TAC rappelle ? (nom + téléphone direct)

## 63. Les niveaux de sévérité S1 à S4 : tableau général

D'après la documentation officielle des services Hi-Care :

| Sévérité | Nom usuel | Fenêtre de couverture | Temps de réponse contractuel* |
|---|---|---|---|
| **S1** | Critique | 24x7 | **30 minutes** |
| **S2** | Majeure | 24x7 | **60 minutes** |
| **S3** | Moyenne | 24x7 (traitement) | **2 heures** |
| **S4** | Faible / demande | 24x7 (traitement) | **Jour ouvré suivant (NBD)** |

*\* Temps entre l'acceptation de la demande par le TAC et le premier contact
d'un ingénieur. Les temps de résolution dépendent de la complexité.*

## 64. S1 — Critique : quand le réseau est à terre

**Définition :** panne avec **impact critique sur l'activité** — réseau ou
environnement hors service.

Exemples :
- L'USG6000 du site principal est HS, tout le site est isolé.
- Les deux membres d'un cluster HA sont tombés.
- Faille de sécurité en cours d'exploitation avérée.

**Ce que Huawei s'engage à faire :**
- Ressources **dédiées**, travail en **24x7 jusqu'à résolution ou
  contournement**.
- Tous les efforts commercialement raisonnables pour fournir un contournement.

**Ce que VOUS devez faire :**
- Ressources disponibles **24x7** (quelqu'un doit décrocher quand le TAC
  rappelle, même à 3h du matin).
- Fournir les infos de diagnostic nécessaires.

**Points de vigilance :**
- Si un contournement est mis en place, la sévérité est **abaissée à S3**
  pour la recherche de la cause racine.
- Si l'ingénieur TAC ne parvient pas à vous joindre **dans l'heure**, la
  sévérité est **temporairement abaissée** jusqu'au rétablissement du contact.
  → En S1, le téléphone d'astreinte doit être sur vous, chargé, avec du réseau.

## 65. S2 — Majeure : dégradation importante

**Définition :** fonctionnalité majeure dégradée, impact significatif mais le
réseau n'est pas totalement à terre.

Exemples :
- Un des deux USG du cluster HA est tombé (le second tient la charge).
- La moitié des AP d'un bâtiment ne montent plus.
- Débit divisé par deux sur un lien critique, sans coupure totale.

**Engagements :** ressources disponibles 24x7, réponse en 60 minutes,
efforts raisonnables pour un contournement.

## 66. S3 — Moyenne : sans impact critique

**Définition :** problème qui **n'affecte pas les fonctions critiques** —
dégradation de capacité, de mesure, ou gêne limitée.

Exemples :
- Un AP isolé sur 50 ne répond plus (couverture redondante).
- Les logs ne remontent plus vers le syslog (le réseau fonctionne).
- Question de configuration sans urgence.

**Engagements :** traitement pendant les heures ouvrées locales jusqu'à
résolution ou contournement, réponse en 2 heures.

## 67. S4 — Faible : demandes et questions

**Définition :** demande d'information, documentation, question de
fonctionnement, anomalie cosmétique.

Exemples :
- « Comment activer telle fonction sur l'AR720 ? »
- Coquille dans une page web d'administration.
- Demande de documentation.

**Engagements :** réponse le jour ouvré suivant (NBD).

## 68. Bien choisir sa sévérité (et ne pas en abuser)

| Bonne pratique | Mauvaise pratique |
|---|---|
| S1 uniquement si l'activité est réellement bloquée | Mettre S1 « pour aller plus vite » sur un S3 |
| Décrire l'impact métier factuellement | Exagérer l'impact pour forcer la priorité |
| Accepter la requalification proposée par le TAC si justifiée | S'obstiner sur S1 quand un contournement existe |

**Pourquoi c'est important :** abuser des S1 décrédibilise vos vrais S1.
Les équipes TAC notent l'historique. Le jour où vous aurez un vrai S1 un
dimanche à 2h du matin, vous voulez que le mot « critique » venant de vous
soit pris au sérieux immédiatement (voir cas vécu n°108).

## 69. Les informations OBLIGATOIRES du ticket

Un ticket sans ces éléments fera des allers-retours. Fournissez **d'emblée** :

1. **Modèle exact** de l'équipement (ex. : `USG6680`, `S310-24P`).
2. **Version logicielle** (ex. : `VRP V600R024C10` — via `display version`).
3. **ESN et/ou numéro de série**.
4. **Contrat de support** (n° Hi-Care, ou « sous garantie standard »).
5. **Description du problème** : quoi, quand, fréquence, reproductible ?
6. **Impact métier** : qui est touché, depuis quand, contournement en place ?
7. **Logs** : `display diagnostic-information` en pièce jointe (section 76).
8. **Topologie** : schéma simplifié du site concerné.
9. **Contact joignable** : nom, téléphone direct, email, fuseau horaire,
   disponibilités.
10. **Actions déjà tentées** : reboot ? changement de câble ? rollback ?

## 70. Template de ticket TAC en anglais (vierge, prêt à copier)

```text
Subject: [S2] USG6000 - IPS license expired, UTM blades down - Site Paris

1. Customer information
   Company: [COMPANY NAME]
   Contact name: [FULL NAME]
   Phone (24/7 reachable): [PHONE NUMBER]
   Email: [EMAIL]
   Time zone / availability: [e.g. CET, available 24/7 for S1/S2]

2. Equipment information
   Product model: [e.g. USG6680]
   Software version: [e.g. VRP V600R024C10 - from "display version"]
   ESN: [from "display esn"]
   Serial Number: [from label or "display device"]
   Support contract: [Hi-Care contract number or "standard warranty"]

3. Problem description
   Summary: [one sentence]
   First occurrence: [date + time with time zone]
   Frequency: [permanent / intermittent - how often / reproducible: yes-no]
   Error messages (exact text):
   [paste exact messages]

4. Business impact
   Impacted users/services: [who / what]
   Workaround in place: [yes - describe / no]
   Requested severity: [S1/S2/S3/S4 + one-line justification]

5. Troubleshooting already performed
   - [action 1 + result]
   - [action 2 + result]

6. Attachments
   - diagnostic-information output: [filename]
   - network topology diagram: [filename]
   - screenshots: [filenames]

7. Requested action
   [e.g. "Please provide a workaround and root cause analysis."]
```

## 71. Template rempli — exemple fictif n°1 (licence UTM)

```text
Subject: [S2] USG6000 - IPS/AV licenses expired, UTM inspection stopped - Site Paris

1. Customer information
   Company: ACME Industries (fictitious)
   Contact name: Jean Dupont (fictitious)
   Phone (24/7 reachable): +33 6 00 00 00 00 (fictitious)
   Email: jean.dupont@example.com (fictitious)
   Time zone / availability: CET, available 24/7 for S1/S2

2. Equipment information
   Product model: USG6680 (fictitious model reference)
   Software version: VRP V600R024C10 (from "display version")
   ESN: 2102311HSL8H6000123 (fictitious)
   Serial Number: 2102311HSL8H6000123 (fictitious)
   Support contract: Hi-Care Standard 9x5xNBD, contract #HC-2026-4451 (fictitious)

3. Problem description
   Summary: IPS, AV and URL filtering blades stopped working this morning.
   First occurrence: 2026-09-27 08:15 CET
   Frequency: permanent
   Error messages (exact text):
   "License of IPS will expire in 0 days" then "The license file has expired."

4. Business impact
   Impacted users/services: 350 users on Paris site without UTM inspection.
   Basic firewalling still works.
   Workaround in place: no - renewal .dat file requested from partner,
   pending reception.
   Requested severity: S2 - security function down on main site, no workaround.

5. Troubleshooting already performed
   - "display license" confirms expiry date 2026-09-26 for IPS/AV/URL features.
   - Rebooted the USG at 08:30 CET - no change.
   - Checked with partner: renewal order in progress, file expected tomorrow.

6. Attachments
   - diagnostic-information output: diag_USG6680_20260927.txt
   - network topology diagram: topo_paris_site.pdf

7. Requested action
   Please confirm whether a temporary license can be issued to cover the gap
   until the renewal file arrives, and provide the procedure.
```

*(Toutes les valeurs ci-dessus sont fictives : remplacez par vos données réelles.)*

## 72. Template rempli — exemple fictif n°2 (matériel)

```text
Subject: [S1] S310-24P - switch not booting, site Lyon warehouse isolated

1. Customer information
   Company: ACME Industries (fictitious)
   Contact name: Marie Martin (fictitious)
   Phone (24/7 reachable): +33 6 11 11 11 11 (fictitious)
   Email: marie.martin@example.com (fictitious)
   Time zone / availability: CET, available 24/7

2. Equipment information
   Product model: S310-24P (fictitious)
   Software version: unknown - device does not boot (last known: V200R021C00)
   ESN: 2102311ABC8H7000456 (fictitious)
   Serial Number: 2102311ABC8H7000456 (fictitious)
   Support contract: Hi-Care Premier 24x7x4, contract #HC-2026-4452 (fictitious)

3. Problem description
   Summary: Switch does not boot since a power outage this morning.
   First occurrence: 2026-09-27 06:05 CET (after mains power restoration)
   Frequency: permanent - stuck at boot, PWR LED red, SYS LED off.
   Error messages (exact text):
   Console shows "BootROM checksum error" then halts. (fictitious message)

4. Business impact
   Impacted users/services: entire Lyon warehouse (80 users, barcode scanners,
   printers) - no network access.
   Workaround in place: no - single switch on this site, no spare on site.
   Requested severity: S1 - full site isolated, critical for morning shipments.

5. Troubleshooting already performed
   - Power-cycled 3 times, tried another power outlet and power cord.
   - Console cable checked on another device - cable OK.
   - No recent configuration change (last change 3 weeks ago).

6. Attachments
   - console boot log: bootlog_S310_lyon.txt
   - photo of LEDs: leds_s310.jpg
   - site topology: topo_lyon_warehouse.pdf

7. Requested action
   Please diagnose and, if hardware failure confirmed, trigger RMA under
   Hi-Care Premier 24x7x4 (4-hour part delivery expected).
```

## 73. Ouvrir le ticket : pas à pas sur le portail

1. Connectez-vous à https://support.huawei.com → **Service Request / TAC**.
2. Sélectionnez le produit et l'équipement enregistré (section 49).
3. Choisissez la **sévérité** (sections 63–68) — soyez factuel.
4. Remplissez le formulaire en collant votre template (section 70).
5. Joignez les pièces : diagnostic-information, topologie, captures.
6. Validez : notez le **numéro de ticket** (format type `SR-XXXX...` —
   format exact **à vérifier sur le portail officiel**).
7. Informez votre équipe et votre astreinte : qui suit le ticket, à quel rythme.

## 74. Suivi du ticket : statuts et SLA

Cycle de vie typique d'un ticket :

```
Ouvert → Accepté par le TAC → En diagnostic → Contournement proposé →
Cause racine identifiée → Résolu → Clôturé (avec votre accord)
```

- **Réponse initiale** : selon la sévérité (30 min / 60 min / 2 h / NBD).
- **Mises à jour** : le TAC doit vous tenir informé ; si 24h sans nouvelles
  sur un S2, relancez (toujours dans le ticket, jamais « en parallèle » par
  un autre canal : un seul fil = pas de perte d'info).
- **Ne fermez jamais un ticket vous-même** tant que la cause racine n'est pas
  comprise ou qu'un contournement stable n'est pas validé en production.

## 75. L'escalade : quand et comment

Escalader = demander un niveau d'attention supérieur. Motifs légitimes :

| Motif | Action |
|---|---|
| SLA de réponse dépassé | Relance formelle dans le ticket + appel hotline en citant le n° de ticket |
| Diagnostic au point mort depuis 48h (S1/S2) | Demander l'intervention d'un expert produit / du R&D (via le TAC) |
| Impact métier aggravé | Requalifier la sévérité à la hausse (S3→S2) avec justification |
| Désaccord sur la cause | Demander une revue par un second ingénieur |

**Comment escalader proprement** (en anglais dans le ticket) :
```text
Escalation request: no progress since [date/time], business impact increasing
([describe]). Requesting senior engineer review and management attention.
Ticket: [number]. Contact 24/7: [name, phone].
```
Toujours factuel, jamais agressif : l'ingénieur en face est votre allié,
pas votre adversaire.

---

# F. COLLECTER LES INFOS POUR LE TAC + GARANTIE

## 76. `display diagnostic-information` : la pièce maîtresse

C'est **la** commande que le TAC vous demandera dans 9 cas sur 10. Elle
agrège en une seule sortie : version, configuration, états des interfaces,
logs, diagnostic matériel.

```
<USG6000> display diagnostic-information
```

- Redirigez vers un fichier : la sortie fait plusieurs milliers de lignes.
- Nommez le fichier : `diag_<modèle>_<site>_AAAAMMJJ.txt`.
- Collectez-la **pendant le problème** si possible (pas 3 jours après
  un reboot qui a tout effacé).

> Si la commande n'existe pas sur votre version, l'équivalent est une
> combinaison de `display version`, `display current-configuration`,
> `display logbuffer`, `display device` — demandez au TAC la liste exacte.

## 77. Exporter les logs : logbuffer et logfile

```
<USG6000> display logbuffer          ← logs en mémoire (volatils !)
<USG6000> display logfile            ← logs persistants (flash)
```

- Le **logbuffer** se vide au reboot : en cas de panne avec redémarrage,
  exportez-le **avant** de rebooter si l'équipement répond encore.
- Filtrez par période : repérez l'heure exacte du début de panne et
  fournissez ±30 minutes autour.
- Exportez en texte brut, pas en capture d'écran (le TAC doit pouvoir
  chercher dedans).

## 78. Les captures et schémas à fournir

| Élément | Format conseillé | Contenu |
|---|---|---|
| Topologie du site | PDF / PNG | Équipements, liens, adresses IP, VLAN |
| Face avant/arrière | Photo nette | LED allumées/éteintes, câblage |
| Étiquette S/N | Photo nette | S/N, modèle, date de fabrication |
| Message d'erreur | Texte copié-collé | Le texte exact, pas une paraphrase |
| Graphique de supervision | PNG | Trafic/CPU avant-pendant-après la panne |

Une photo floue de l'étiquette = un aller-retour de 24h avec le TAC.
Prenez 30 secondes pour la refaire nette.

## 79. Décrire une panne efficacement : la méthode

Structure imposée à votre équipe pour tout compte-rendu de panne :

1. **Quoi** : le symptôme observable (pas l'interprétation).
   - ✅ « Les utilisateurs du VLAN 20 n'obtiennent plus d'IP depuis 8h15. »
   - ❌ « Le DHCP est cassé. » (interprétation, peut-être fausse)
2. **Quand** : date/heure précise + fuseau, première occurrence.
3. **Où** : quel équipement, quel site, quel segment.
4. **Fréquence** : permanent / intermittent (toutes les X minutes) /
   lié à un événement (coupure électrique, mise à jour...).
5. **Changements récents** : config, firmware, câblage, coupure de courant
   dans les 7 derniers jours.
6. **Ce qui a été tenté** et le résultat de chaque action.

## 80. La règle des 5W adaptée au TAC

| W | Question | Exemple de réponse |
|---|---|---|
| What | Que se passe-t-il ? | Plus de distribution DHCP sur VLAN 20 |
| When | Depuis quand ? | 2026-09-27 08:15 CET, permanent |
| Where | Où ? | S310-BatA, Lyon, ports 1-12 |
| Who | Qui est impacté ? | 40 utilisateurs du bâtiment A |
| Why (piste) | Qu'est-ce qui a changé ? | Mise à jour VRP hier soir 22h |

Rédigez ces 5 lignes **avant** d'appeler : l'ingénieur TAC vous les
demandera de toute façon, autant les avoir prêtes.

---

## 81. Garantie : les types chez Huawei

Deux grands régimes, cumulables dans le temps :

| Régime | Nature | Durée typique |
|---|---|---|
| **Garantie standard** | Incluse à l'achat, définie par la politique de garantie Huawei par gamme | 1 an à vie limitée selon produits (détails section 82) |
| **Hi-Care** | Contrat de support payant (« maintenance », extension de garantie) | 1 à 3 ans, renouvelable |

Logique d'achat : la garantie standard couvre le matériel ; **le TAC complet
et les délais de remplacement courts exigent Hi-Care** (voir section 84).

## 82. La garantie standard : contenu par gamme

D'après la politique de garantie Huawei Enterprise (documents publics) :

| Gamme | Garantie standard constatée* |
|---|---|
| Certains switches (S5710HI, S5720HI, S6700, S7700...) | Garantie à vie limitée (10 ans max.), pièces expédiées à J+10 ouvrés |
| Certains switches (S2700, S3700, S5700...) | Garantie à vie limitée (10 ans max.), pièces expédiées le jour ouvré suivant |
| Routeurs AR, IoT, sécurité, certains switches/AP WLAN | 1 an, avec variantes (retour pour réparation, etc.) |
| Mises à jour logicielles | 1 an inclus en général |

*\* Politique détaillée par référence dans la « Warranty List » officielle —
**à vérifier sur le portail officiel** pour chaque modèle de votre parc
(AP361, AP761, S310, AR720, USG6000). Les conditions varient selon les
régions.*

Modes de remplacement sous garantie standard :
- **AR (Advance Replacement)** : la pièce de remplacement est expédiée avant
  (ou sans attendre) le retour de la défectueuse.
- **RFR (Return For Repair)** : vous renvoyez d'abord, réparation sous
  ~30 jours ouvrés après réception au centre de pièces.

## 83. Hi-Care : les paliers

Hi-Care est le portefeuille de support « branded » Huawei pour l'entreprise.
Paliers constatés dans la documentation officielle :

| Palier | Fenêtre | Pièces de rechange |
|---|---|---|
| **Basic** | 9x5 (9h-18h, lun-ven hors fériés) | Expédiées le jour ouvré suivant (**NBD-S** = NBD Ship) |
| **Standard** | 9x5 | **Reçues** le jour ouvré suivant (**NBD**) si RMA avant 15h00 locales |
| **Premier** | 24x7 pour P1/P2 ; 9x5 pour P3/P4 | **4 heures** (P1/P2) après génération du n° RMA ; NBD (P3/P4) |
| **Onsite Standard** | 9x5 | NBD + intervention sur site 9x5 |
| **Onsite Premier** | 24x7 | 4h + intervention sur site 24x7 |

Tous les paliers incluent : **TAC 24x7**, support en ligne (self-help),
mises à jour logicielles système.

## 84. Tableau comparatif : que choisir pour quel équipement ?

| Équipement (exemple) | Enjeu | Palier recommandé | Justification |
|---|---|---|---|
| USG6000 site principal | Critique, pas de redondance | **Premier 24x7x4** | 4h de remplacement, TAC prioritaire |
| USG6000 site secondaire (HA) | Important, redondé | Standard 9x5xNBD | Le second membre tient la charge |
| S310 bâtiment | Moyen, redondance partielle | Standard 9x5xNBD | NBD acceptable si maillage |
| AP361 / AP761 | Faible unitaire | Basic ou garantie standard | Un AP en panne = gêne locale ; stock de spare (section 112) |
| AR720 agence distante | Moyen | Standard 9x5xNBD | Agence isolée : NBD suffisant si 4G de secours |
| eSight (serveur) | Supervision | Standard | Pas de coupure métier directe |

**Règle budgétaire :** le coût Hi-Care est en général de l'ordre d'un
pourcentage annuel du prix du matériel (**tarifs à vérifier auprès du
partenaire**). Budgétez-le dès l'achat, pas après la première panne.

## 85. Les délais pièces : comprendre NBD-S, NBD et la règle des 15h00

- **NBD-S (Next Business Day - Ship)** : la pièce **part** de chez Huawei le
  jour ouvré suivant. Ajoutez le transit : réception J+2 ou J+3 en pratique.
- **NBD (Next Business Day)** : la pièce **arrive** le jour ouvré suivant,
  **à condition que le n° RMA soit généré avant 15h00 locales**. Après 15h00 :
  le RMA est enregistré le jour ouvré suivant, la pièce arrive le surlendemain.
- **4h (Premier, P1/P2)** : la pièce arrive **dans les 4 heures** suivant la
  génération du n° RMA, 24x7.

**Conséquence opérationnelle :** un ticket S1 ouvert à 16h30 avec un contrat
Standard = pièce le surlendemain, pas le lendemain. D'où l'importance du
palier pour les sites critiques (cas vécu n°112).

## 86. Début et fin de service : la règle des 90 jours

D'après la documentation Hi-Care, si aucune date de début ne figure au bon
de commande :

- Service vendu **avec** le produit : démarrage le **90e jour après
  l'expédition** du produit par Huawei.
- Si Huawei fournit aussi l'**installation / mise en service** : démarrage
  à la **date d'acceptation client**.
- **Renouvellement** : démarrage le lendemain de la fin du contrat précédent
  (pas de trou de couverture si renouvelé à temps).

> **Vérifiez vos contrats** : une installation faite au jour 30 avec un
> démarrage de service au jour 90 = 60 jours de « garantie standard seule ».
> Négociez la date de début sur le bon de commande.

## 87. Ce qui est couvert / non couvert

**Couvert en général :**
- Panne matérielle non provoquée (composant défectueux).
- Bugs logiciels (correctifs via TAC).
- Remplacement à l'identique ou équivalent.

**Non couvert en général :**
- Dommages physiques (choc, eau, surtension sans parafoudre...).
- Modifications non autorisées, firmware tiers.
- Consommables (ventilateurs usés ? selon contrat — **à vérifier**).
- Perte de données de configuration (votre responsabilité : sauvegardes !).
- Interventions dues à une mauvaise utilisation documentée.

**En cas de doute :** le TAC qualifie la panne ; s'il conclut à un dommage
exclu, le RMA est refusé et un devis de réparation est proposé (cas vécu n°107).

## 88. Vérifier la garantie : procédure annuelle

1. Exporter la liste des S/N depuis le portail (section 50).
2. Pour chaque équipement : noter fin de garantie + palier Hi-Care.
3. Marquer en rouge les fins < 6 mois.
4. Chiffrer les renouvellements avec le partenaire.
5. Présenter au budget N+1.

Modèle de ligne de suivi :
```
S310-BatA | S/N 21023... (fictif) | Garantie std + Hi-Care Standard
| Fin : 2028-06-30 | Statut : OK | Action : renouveler avant 2028-03-30
```

## 89. Renouveler / étendre la garantie et le Hi-Care

- Le renouvellement se commande **avant** l'expiration (idéalement J-90,
  comme les licences).
- Un contrat expiré depuis longtemps peut nécessiter une **remise en
  conformité** (inspection, voire refus) — **conditions à vérifier auprès
  du partenaire**.
- À chaque renouvellement : exiger l'avenant écrit avec les dates exactes
  de début/fin et le palier.

## 90. Garantie et achats : les clauses à faire figurer au bon de commande

- [ ] Références exactes des contrats Hi-Care (palier, durée, sites couverts).
- [ ] Date de début de service explicite.
- [ ] Liste des pièces de rechange critiques couvertes.
- [ ] Engagement de délai de génération du n° RMA.
- [ ] Contact support dédié du partenaire (nom, téléphone).
- [ ] Clause de transfert de licence en cas de RMA (section 32).

---

# G. PROCÉDURE RMA

## 91. RMA : définition et vocabulaire

**RMA** (Return Material Authorization) = l'autorisation formelle, avec un
**numéro RMA**, d'échanger un matériel défectueux. Sans numéro RMA, aucun
échange n'est possible : c'est la clé de voûte de tout le processus.

| Terme | Signification |
|---|---|
| **Numéro RMA** | Référence unique de l'échange, générée par Huawei après validation du diagnostic |
| **AR** (Advance Replacement) | La pièce neuve est expédiée avant le retour de la défectueuse |
| **RFR** (Return For Repair) | Vous renvoyez d'abord ; réparation sous ~30 jours ouvrés |
| **DOA** (Dead On Arrival) | Matériel défectueux dès la réception → procédure accélérée |
| **Faulty part** | La pièce défectueuse à retourner |
| **Replacement part** | La pièce de remplacement reçue |

## 92. Étape 1 — Constater et qualifier la panne

Avant tout appel :

1. [ ] Décrire le symptôme (méthode section 79).
2. [ ] Vérifier les causes triviales : alimentation, câble, SFP, température
      (`display environment`, `display device`), config récente.
3. [ ] Collecter `display diagnostic-information` (section 76).
4. [ ] Prendre en photo les LED et l'étiquette S/N.
5. [ ] Déterminer la sévérité (S1–S4, sections 63–68) selon l'impact réel.

**Ne déclarez pas « matériel HS » trop vite** : ~30 % des RMA demandés
s'avèrent être des problèmes de configuration ou d'environnement
(alimentation, chaleur). Le TAC le vérifiera de toute façon à l'étape 3.

## 93. Étape 2 — Ouvrir le ticket TAC

- Ouvrez le ticket selon la partie E (template section 70).
- Précisez que vous suspectez une **panne matérielle** et demandez un
  diagnostic en vue d'un RMA.
- Joignez : diagnostic-information, photos LED, logbuffer autour de la panne.

## 94. Étape 3 — Le diagnostic à distance avec le TAC

L'ingénieur TAC va :

1. Analyser vos logs et la sortie diagnostic.
2. Vous faire exécuter des tests complémentaires (boucles, changement de
   port/SFP, boot sur une autre partition...).
3. Écarter les causes logicielles et environnementales.
4. **Statuer** : panne matérielle confirmée → passage à l'étape 4 ;
   cause non matérielle → poursuite du diagnostic logiciel.

**Votre rôle :** exécuter précisément ce qui est demandé, noter les résultats,
rester joignable. Chaque test non fait = 24h de retard.

## 95. Étape 4 — Validation du remplacement et numéro RMA

Quand la panne matérielle est confirmée :

1. Le TAC (ou le partenaire selon le circuit) génère le **numéro RMA**.
2. Vous recevez : le n° RMA, la référence de la pièce expédiée, le délai
   annoncé selon votre palier (section 85), les instructions de retour.
3. **Notez le n° RMA dans votre registre** et communiquez-le à l'équipe.

> Le délai contractuel (NBD, 4h...) court à partir de la **génération du
> n° RMA**, pas de l'ouverture du ticket. D'où l'intérêt d'un diagnostic
> rapide et bien documenté.

## 96. Étape 5 — Expédition de la pièce de remplacement

- **Standard 9x5xNBD** : RMA généré avant 15h00 → pièce reçue le jour ouvré
  suivant ; après 15h00 → enregistrement le jour ouvré suivant, réception
  le surlendemain.
- **Premier 24x7x4 (P1/P2)** : pièce reçue sous 4h, 24x7.
- **Basic (NBD-S)** : pièce expédiée le jour ouvré suivant (réception J+2/J+3).
- Suivez le tracking communiqué ; prévenez le site de réception.

## 97. Étape 6 — Réception et vérification du matériel de remplacement

À la réception, **avant** de renvoyer le défectueux :

- [ ] Contrôler le colis (choc, humidité) et prendre des photos.
- [ ] Vérifier la référence reçue = la référence attendue.
- [ ] Relever le **nouvel ESN / S/N** (`display esn`).
- [ ] Tester sommairement (boot, quelques ports) avant mise en production.
- [ ] Si **DOA** (défectueux à réception) : photo + ticket immédiat, ne pas
      renvoyer l'ancien avant d'avoir une solution (cas vécu n°106).

## 98. Étape 7 — Retour du matériel défectueux

- Utilisez l'**emballage de la pièce de remplacement** (prévu pour).
- Joignez : n° RMA **lisible** sur le colis et dans le colis, fiche de
  description de la panne.
- Expédiez via le transporteur / l'adresse indiqués (ne pas improviser).
- Conservez la **preuve de dépôt** jusqu'à confirmation de réception par Huawei.
- **Délai de retour** : en général sous 10–15 jours (**à vérifier sur vos
  instructions RMA**). Au-delà : facturation de la pièce non retournée possible.

## 99. Étape 8 — Transfert de licence et restauration de configuration

Sur l'équipement de remplacement :

1. [ ] Restaurer la **configuration** (sauvegarde — vous en avez une, n'est-ce pas ?).
2. [ ] Transférer les **licences** vers le nouvel ESN (section 32).
3. [ ] Vérifier `display license` : fonctions + dates OK.
4. [ ] Mettre à jour le registre (ancien ESN → « retourné », nouvel ESN → actif).
5. [ ] Mettre à jour l'enregistrement sur le portail (nouveau S/N sur le site).
6. [ ] Supervision : vérifier que l'équipement remonte correctement.

## 100. RMA et sécurité des données

- Les équipements réseau contiennent votre **configuration** (mots de passe,
  clés VPN, certificats). Avant retour : faites un **reset usine** si
  l'équipement démarre encore (`reset saved-configuration`, puis reboot —
  commande exacte **à vérifier sur le portail officiel** pour votre modèle).
- Si l'équipement ne démarre plus : signalez-le au TAC ; demandez la
  procédure de **non-retour pour raison de sécurité** si votre politique
  l'exige (certains contrats prévoient la conservation/destruction locale
  du support — **à vérifier dans votre contrat**).
- Ne laissez jamais un disque/SSD contenant des données partir sans traçabilité.

---

# H. CAS VÉCUS COMMENTÉS

> Les cas ci-dessous sont des **scénarios types reconstitués** à partir de
> situations classiques du support réseau d'entreprise. Les noms, dates et
> valeurs sont fictifs. Chacun se termine par la leçon opérationnelle.

## 101. Cas n°1 — Licence UTM expirée un vendredi soir

**Situation.** Vendredi 18h42 : l'USG6000 du siège ne fait plus d'inspection
IPS. Les logs indiquent `The license file has expired`. La licence 1 an,
achetée avec l'équipement, expirait ce jour. Personne n'avait vu l'alerte
J-30 (partie dans les spams de la boîte générique).

**Gestion.** S2 ouvert au TAC le vendredi soir ; le partenaire, injoignable
avant lundi, ne peut générer le `.dat` de renouvellement que le lundi matin.
Le TAC confirme qu'aucune licence temporaire ne peut être délivrée sans
l'aval commercial. Le site passe le week-end sans inspection UTM (firewall
de base maintenu, navigation restreinte par consigne).

**Leçon.** 1) Les alertes d'expiration doivent partir vers une **boîte
surveillée** + SMS d'astreinte, pas une boîte générique. 2) Le script de
contrôle nocturne (`display license`, section 27) aurait alerté à J-90.
3) Envisager le renouvellement **3 ans** pour lisser le risque.

## 102. Cas n°2 — RMA urgent avant un audit

**Situation.** À J-5 d'un audit de sécurité, le second USG du cluster HA
rend l'âme (panne d'alimentation). Le site n'est plus redondé. L'auditeur
va forcément le relever.

**Gestion.** Ticket S2 (le site fonctionne sur le membre restant : pas de S1,
mais l'impact potentiel justifie S2). Diagnostic TAC en 3h, panne matérielle
confirmée, n° RMA généré à 14h10 → contrat Standard 9x5xNBD → pièce reçue
le lendemain 10h. Remplacement, restauration de config, transfert de licence
vers le nouvel ESN (section 32) : HA rétabli à J-3. L'audit se passe bien,
avec le rapport RMA en pièce justificative.

**Leçon.** 1) La règle des **15h00** (section 85) : un RMA généré à 14h10
change tout vs 15h10. 2) Garder le **dossier RMA complet** : c'est une preuve
de maîtrise pour l'auditeur. 3) Sur un cluster HA, chaque membre doit avoir
ses licences (section 41) — vérifié ici avant l'audit, ouf.

## 103. Cas n°3 — Le TAC demande des logs introuvables

**Situation.** Ticket S3 sur des déconnexions Wi-Fi intermittentes (AP761).
Le TAC demande le `display diagnostic-information` **au moment du problème**
et les logs du contrôleur sur la plage horaire. L'équipe n'a que des logs
d'il y a 3 jours, le logbuffer a tourné depuis.

**Gestion.** Mise en place d'une collecte proactive : export syslog permanent
vers le serveur de logs, script qui fige le diagnostic-information dès
qu'une alerte remonte. Deuxième occurrence capturée « en flagrant délit » :
le TAC identifie un bug connu, patch fourni.

**Leçon.** 1) Sans logs **horodatés au moment des faits**, le TAC ne peut
rien faire : c'est parole contre silence. 2) La **centralisation syslog**
n'est pas une option, c'est un prérequis du support. 3) Documenter la
procédure de collecte (sections 76–78) et la tester **avant** la panne.

## 104. Cas n°4 — ESN illisible sur l'étiquette

**Situation.** Un S310 à remplacer : l'étiquette au dos est effacée (chaleur
+ poussière du local technique). Le `display esn` est inaccessible : le
switch ne boote plus. Impossible de générer le fichier de licence du
remplacement sans ESN... qui est celui de l'**ancien** équipement, pas du
nouveau — rappel utile : pour le nouvel équipement, c'est **son** ESN qu'il
faut (il boote, lui).

**Gestion.** En réalité le blocage venait d'ailleurs : le technicien cherchait
l'ESN de l'ancien pour « transférer » la licence, alors qu'il fallait
simplement relever l'ESN du **nouvel** équipement (`display esn` OK) et
demander la réémission du droit. Pour l'ancien ESN (traçabilité du retour),
le S/N partiel + la photo de l'étiquette + le bon de livraison d'origine
ont suffi au partenaire.

**Leçon.** 1) À la réception : **photographier toutes les étiquettes** et
archiver (checklist section 114). 2) Enregistrer les S/N sur le portail
dès la réception (section 49) : le portail devient la source de vérité quand
l'étiquette meurt. 3) Comprendre le sens du transfert (section 32) évite de
chercher le mauvais ESN.

## 105. Cas n°5 — Firmware non téléchargeable sans contrat

**Situation.** Faille critique publiée sur la version VRP des AR720. L'équipe
veut patcher. Sur le portail : le firmware cible est visible mais le
téléchargement est **refusé** — le contrat logiciel a expiré il y a 4 mois.

**Gestion.** Deux options : 1) renouveler le contrat logiciel (devis
partenaire, 48h), 2) contournement temporaire (ACL, désactivation du service
vulnérable) en attendant. L'équipe choisit le contournement documenté +
renouvellement en urgence. Le patch est appliqué à J+3.

**Leçon.** 1) Le droit de télécharger les firmwares est **contractuel**
(section 51), pas technique. 2) Les bulletins de sécurité doivent déclencher
une vérification **immédiate** des droits de téléchargement. 3) Maintenir les
contrats logiciels comme les licences : calendrier partagé (section 34).

## 106. Cas n°6 — Pièce DOA à la réception

**Situation.** RMA d'un S310 : la pièce de remplacement arrive, le technicien
la monte... et elle ne boote pas non plus (emballage éventré, choc visible
sur un coin).

**Gestion.** Photos immédiates du colis et de l'équipement, ticket rouvert
avec mention **DOA**, l'ancien équipement (toujours sur site) n'est **pas**
renvoyé. Le TAC valide le DOA en 2h, nouvelle pièce expédiée en NBD. Leçon
apprise : la vérification de la section 97 (tester avant de renvoyer
l'ancien) a évité de se retrouver sans aucun switch.

**Leçon.** 1) Ne renvoyez **jamais** le défectueux avant d'avoir testé le
remplacement. 2) Photographiez systématiquement les colis à réception.
3) Un DOA se traite en priorité : signalez-le dans l'heure.

## 107. Cas n°7 — RMA refusé : hors garantie et dommage exclu

**Situation.** Un AP761 ne s'allume plus après un orage. Ticket ouvert,
diagnostic TAC : traces de **surtension** sur l'alimentation (pas de
parafoudre sur le site), équipement hors garantie standard de toute façon.
RMA refusé : dommage exclu + pas de contrat.

**Gestion.** Devis de réparation proposé par Huawei (montant dissuasif) ;
décision : achat d'un AP neuf via le partenaire (délai 1 semaine) +
installation d'un **parafoudre** et vérification de la terre sur le site.
Le coût total (AP + parafoudre + déplacement) est imputé en « leçon ».

**Leçon.** 1) La garantie ne couvre pas les dommages externes (section 87) :
protégez les sites (parafoudre, terre < 5 ohms — voir le guide onduleurs).
2) Un équipement hors contrat = RMA impossible : c'est un achat, pas un échange.
3) Après un orage, inspectez **tout** le site, pas seulement l'équipement HS.

## 108. Cas n°8 — Sévérité S1 requalifiée en S3

**Situation.** Panne du lien principal d'une agence (AR720). Le technicien
d'astreinte ouvre un **S1** « pour aller plus vite ». Le TAC constate : le
lien 4G de secours fonctionne, l'agence travaille (dégradé). Requalification
en **S3**, réponse en 2h au lieu de 30 min. Le technicien s'énerve, le ton
monte.

**Gestion.** Le responsable (prévenu par l'astreinte) rappelle le cadre :
le S1 exige un **impact critique réel** (section 64). Le lien est rétabli
via le TAC en 4h en S3. En débrief : on ne « gonfle » pas la sévérité.

**Leçon.** 1) Une sévérité abusive **retarde** le traitement (requalification
+ friction) au lieu de l'accélérer. 2) Décrivez l'impact **factuellement**,
laissez le TAC qualifier. 3) Vos S1 doivent rester crédibles (section 68) :
c'est un capital.

## 109. Cas n°9 — Renouvellement Hi-Care oublié

**Situation.** Le contrat Hi-Care des USG expire un 31 décembre. L'oubli est
découvert le 15 janvier, lors d'une panne mineure. Le TAC ouvre quand même le
ticket (bonne volonté) mais : pas de SLA contractuel, pas de téléchargement
de patch, et le renouvellement « après expiration » exige une procédure de
remise en conformité avec inspection.

**Gestion.** Renouvellement en urgence (surcoût + délai), panne traitée en
« best effort ». Mise en place du calendrier partagé (section 34) avec
alertes J-90/J-60/J-30 **et** responsable nommé.

**Leçon.** 1) Un contrat expiré = des droits perdus immédiatement, même si
le TAC fait un geste commercial ponctuel. 2) Le renouvellement après
expiration coûte plus cher et prend plus de temps. 3) Nommez un
**responsable** des échéances, pas « l'équipe ».

## 110. Cas n°10 — Transfert de licence oublié après RMA

**Situation.** RMA d'un USG6000 parfaitement exécuté : pièce reçue en NBD,
config restaurée, tout fonctionne... sauf l'IPS, inactif. Personne n'a
transféré la licence vers le nouvel ESN (section 32). Découvert 3 semaines
plus tard lors d'un contrôle.

**Gestion.** Demande de transfert en urgence au partenaire (24h), activation
du nouveau `.dat`, vérification `display license`. Le site est resté 3
semaines sans inspection UTM sans que personne ne s'en aperçoive.

**Leçon.** 1) Ajoutez le transfert de licence à la **checklist RMA**
(étape 8, section 99) : c'est une case à cocher, pas une option. 2) Après tout
RMA : `display license` systématique. 3) La supervision doit alerter sur
l'état des licences, pas seulement sur le ping.

## 111. Cas n°11 — eKit : qui appeler quand ça ne répond plus ?

**Situation.** Un AP361 eKit ne diffuse plus. Le technicien appelle la
hotline Huawei : « pour la gamme eKit, merci de contacter votre partenaire
distributeur ». Il appelle le partenaire : « envoyez-nous le diagnostic par
email ». 48h de flottement, chacun renvoyant vers l'autre.

**Gestion.** Le chef de service clarifie le circuit (section 118) : pour
eKit, le **partenaire est le point d'entrée unique** (niveau 1), qui escalade
vers Huawei si nécessaire. Il fait inscrire ce circuit dans le contrat de
maintenance avec des **délais de réponse du partenaire** écrits noir sur blanc.

**Leçon.** 1) eKit = circuit partenaire d'abord (voir partie J). 2) Faites
écrire les délais **du partenaire** dans le contrat : « le partenaire répond
sous Xh ». 3) Conservez quand même vos accès au portail Huawei (docs,
firmwares selon droits).

## 112. Cas n°12 — Panne un jour férié : NBD vs 4h

**Situation.** Jeudi férié, 10h : le S310 d'un bâtiment tombe (panne
matérielle confirmée par le TAC à 11h, n° RMA généré à 11h30). Contrat
**Standard 9x5xNBD**. Le technicien attend la pièce « demain ». Erreur :
vendredi n'est pas férié mais le RMA a été généré un jour férié → enregistré
le vendredi → pièce reçue le **lundi**. Le bâtiment reste sans réseau 4 jours.

**Gestion.** Contournement d'urgence : un switch de secours (spare froid,
acheté d'avance) est configuré et monté le jeudi après-midi. Leçon intégrée
au plan : pour les sites sans redondance, **un spare froid sur étagère**
coûte moins cher que 4 jours d'arrêt.

**Leçon.** 1) Les jours fériés **ne sont pas** des jours ouvrés : NBD après
un férié = +2-3 jours. 2) Pour les équipements en contrat Standard sur site
non redondé : prévoyez un **spare froid** (switch pré-configuré sous
emballage). 3) Les sites vraiment critiques méritent le palier **Premier
24x7x4** (section 84).

---

# I. CHECKLISTS ET CONTACTS

## 113. Checklist « avant d'acheter »

- [ ] Devis détaillé : références, quantités, **licences incluses vs
      optionnelles**, durées de chaque licence.
- [ ] **Proof of Entitlement** promise par écrit (délai de fourniture).
- [ ] Palier Hi-Care choisi **par équipement** (tableau section 84), avec
      dates de début explicites (section 86).
- [ ] Clause de transfert de licence en cas de RMA.
- [ ] Contact support nommé chez le partenaire (nom, téléphone direct).
- [ ] Délais de réponse du partenaire écrits (surtout pour eKit).
- [ ] Vérification : le modèle commandé est-il bien celui dont la doc et les
      licences existent sur le portail (pas de fin de vie imminente) ?

## 114. Checklist « à la réception du matériel »

- [ ] Contrôle colis + photos (chocs, humidité).
- [ ] Vérifier références livrées = bon de commande.
- [ ] **Photographier toutes les étiquettes** (S/N, ESN, modèle) et archiver.
- [ ] Relever `display esn` / `display version` au premier boot.
- [ ] **Enregistrer les S/N sur support.huawei.com** (section 49).
- [ ] Réclamer immédiatement toute **Proof of Entitlement** manquante.
- [ ] Générer et activer les licences (partie B).
- [ ] Vérifier `display license` : fonctions + dates.
- [ ] Inscrire chaque licence au **registre** (section 38).
- [ ] Inscrire chaque fin de garantie/contrat au **calendrier** (section 34).
- [ ] Tester le matériel avant mise en production (non-DOA).
- [ ] Sauvegarder la configuration initiale.

## 115. Checklist « annuelle » (revue administrative)

- [ ] Audit licences : registre vs `display license` réels (section 40).
- [ ] Lister les expirations (licences + contrats) des 12 prochains mois.
- [ ] Vérifier les garanties par S/N sur le portail (section 88).
- [ ] Chiffrer le budget renouvellements N+1, présenter à la direction.
- [ ] Vérifier les accès au portail (départs, nouveaux arrivants).
- [ ] Tester la procédure de collecte de logs (sections 76–78) sur un
      équipement : le jour de la panne n'est pas le jour de l'apprentissage.
- [ ] Relire les contrats partenaires : délais, contacts, toujours d'actualité ?
- [ ] Contrôler l'état du stock de spares froids.

## 116. Checklist « avant d'ouvrir un ticket TAC »

- [ ] Modèle, version VRP, ESN/S/N, n° de contrat : réunis.
- [ ] `display diagnostic-information` collecté **pendant** le problème.
- [ ] Logbuffer exporté (avant tout reboot).
- [ ] Base de connaissances consultée (section 58).
- [ ] Description en 5W rédigée (section 80).
- [ ] Sévérité choisie factuellement (sections 63–68).
- [ ] Template anglais rempli (section 70).
- [ ] Contact joignable 24/7 désigné (nom + téléphone direct).
- [ ] Équipe/astreinte prévenue du n° de ticket dès ouverture.

## 117. Contacts utiles (template à compléter)

```text
PARTENAIRE / INTÉGRATEUR
  Société : ............................ [À COMPLÉTER]
  Commercial : ......................... [À COMPLÉTER] (tél. : ............)
  Support N1 : ......................... [À COMPLÉTER] (tél. : ............)
  Email support : ...................... [À COMPLÉTER]
  Délai de réponse contractuel : ....... [À COMPLÉTER]

HUAWEI TAC (canaux officiels — numéros et emails exacts par région
  à vérifier sur https://support.huawei.com > Contact)
  Hotline : ............................ [À COMPLÉTER]
  Email : .............................. [À COMPLÉTER]
  Portail tickets : https://support.huawei.com (Service Request)

CONTRATS
  Hi-Care USG6000 site principal : n° ............, palier ............, fin le ............
  Hi-Care USG6000 site secondaire : n° ............, palier ............, fin le ............
  [À COMPLÉTER pour chaque équipement critique]

EN INTERNE
  Responsable licences/contrats : ..... [À COMPLÉTER]
  Astreinte réseau : ................... [À COMPLÉTER]
  Boîte email alertes d'expiration : ... [À COMPLÉTER]
```

> **Ne notez jamais de mots de passe dans ce document.** Les accès portail
> sont nominatifs (section 48).

---

# J. SPÉCIFICITÉS EKIT

## 118. eKit : le circuit partenaire, pas le direct Huawei

La gamme **eKit** est la marque « distribution » de Huawei pour les PME :
switches, routeurs, AP (AP361, AP761), passerelles de sécurité, IdeaHub...
Le modèle commercial est **100 % indirect** : Huawei vend aux distributeurs,
qui vendent aux partenaires, qui vendent et supportent le client final.

**Conséquence n°1 : le circuit de support.**

```
Client final (vous)
    │ 1. Appel / ticket
    ▼
Partenaire / intégrateur local  ← VOTRE point d'entrée unique (niveau 1)
    │ 2. Escalade si nécessaire
    ▼
Distributeur agréé eKit         ← niveau 2, stocks, RMA logistique
    │ 3. Escalade si nécessaire
    ▼
Huawei                          ← niveau 3 (via le canal distribution)
```

- N'appelez pas directement la hotline Huawei Enterprise pour un produit
  eKit : on vous redirigera vers votre partenaire (cas vécu n°111).
- En revanche, le portail https://support.huawei.com reste utile pour la
  **documentation** et (selon droits) les **firmwares**.
- Les outils eKit (application mobile / cloud eKit) simplifient la gestion
  courante : utilisez-les pour le niveau 0 (diagnostic de base).

**Conséquence n°2 : exigez un contrat de support écrit avec le partenaire.**

| Clause | Pourquoi |
|---|---|
| Délai de réponse du partenaire (ex. : 4h ouvrées) | Sans écrit, c'est « dès que possible » |
| Procédure d'escalade vers le distributeur/Huawei | Savoir qui fait quoi, en combien de temps |
| Gestion des RMA eKit (qui porte le colis, délais) | Éviter le ping-pong du cas n°111 |
| Fourniture des preuves de droit de licence | Indispensable (partie B) |
| Contacts nommés + suppléants | Pas de « M. X est en congés, rappelez » |

## 119. eKit : garantie, licences, firmwares — ce qui change (et ce qui ne change pas)

| Sujet | Spécificité eKit |
|---|---|
| **Garantie matérielle** | Assurée via le circuit distribution ; enregistrez quand même les S/N sur le portail Huawei. Durées **à vérifier sur le portail officiel** par modèle. |
| **RMA** | Déclenché via le partenaire (qui gère la logistique avec le distributeur). Gardez les mêmes réflexes : photos, n° de dossier, test du remplacement avant retour (partie G). |
| **Licences** | Mêmes principes (ESN, `.dat`, portail). Les AP eKit n'ont en général pas de licence unitaire (section 12). |
| **Firmwares** | Via le partenaire et/ou le portail selon droits ; l'application eKit peut proposer les mises à jour directement. |
| **TAC direct** | Non : le partenaire est le TAC de fait (niveau 1). S'il est compétent et contractuellement engagé, c'est même plus rapide. |
| **Documentation** | Portail Huawei + ressources eKit dédiées (guides d'installation simplifiés, très bien faits pour les PME). |

**Recommandation de chef de service :** traitez votre partenaire eKit comme
une extension de votre équipe — avec des SLA écrits. Un bon partenaire eKit
vaut mieux qu'un mauvais accès direct ; un partenaire sans SLA écrite est un
risque (cas n°111).

---

# K. RÉFÉRENCES

## 120. Glossaire

| Terme | Définition |
|---|---|
| **AR (Advance Replacement)** | La pièce de remplacement est expédiée avant le retour de la défectueuse. |
| **Batch Import** | Génération de fichiers de licence en masse sur le portail (template + ESN). |
| **Commissioning license** | Licence temporaire (~30 jours) pour mise en service et tests. |
| **Co-terming** | Alignement des dates d'expiration de plusieurs contrats/licences. |
| **.dat** | Fichier de licence chiffré Huawei, lié à un ESN. |
| **Diagnostic-information** | Commande agrégeant version, config, états et logs pour le TAC. |
| **DOA (Dead On Arrival)** | Matériel défectueux dès réception. |
| **EID (Entitlement ID)** | Identifiant de droit d'utilisation d'une licence. |
| **ESDP** | Plateforme de distribution électronique des logiciels/licences Huawei. |
| **ESN** | Equipment Serial Number : identifiant logique lié aux licences. |
| **GSC** | Global Service Center : centre de services Huawei (ex. : Europe). |
| **HA** | Haute disponibilité (actif/standby). |
| **HedEx** | Lecteur de documentation électronique Huawei (fichiers .hdx). |
| **Hi-Care** | Portefeuille de contrats de support Huawei pour l'entreprise. |
| **LAC** | License Authorization Code (selon gammes). |
| **Logbuffer** | Journal en mémoire vive (volatil au reboot). |
| **Logfile** | Journal persistant en flash. |
| **NBD** | Next Business Day : pièce reçue le jour ouvré suivant (si RMA avant 15h). |
| **NBD-S** | NBD Ship : pièce expédiée le jour ouvré suivant. |
| **OPEX** | Dépenses d'exploitation (dont renouvellements de licences). |
| **P1–P4 / S1–S4** | Niveaux de priorité/sévérité des tickets TAC. |
| **PN** | Part Number : référence commerciale. |
| **Proof of Entitlement** | Document prouvant le droit : EID + mot de passe d'activation. |
| **RFR (Return For Repair)** | Retour d'abord, réparation sous ~30 jours ouvrés. |
| **RMA** | Return Material Authorization : autorisation + n° d'échange. |
| **RTU** | Right To Use : licence d'activation d'une capacité (ex. : ports 100G). |
| **S/N** | Numéro de série physique du matériel. |
| **SLA** | Engagement de niveau de service (délais contractuels). |
| **Spare froid** | Équipement de secours stocké, pré-configuré, non sous tension. |
| **TAC** | Technical Assistance Center : support technique Huawei. |
| **Temporary license** | Licence temporaire (~60 jours) : démo, avant-vente, secours. |
| **UTM** | Unified Threat Management : lames IPS/AV/URL/antispam des USG. |
| **VRP** | Versatile Routing Platform : OS des équipements réseau Huawei. |
| **WAC** | Contrôleur WLAN (gestion centralisée des AP). |

## 121. Quiz — 10 questions

1. Un fichier `.dat` généré pour l'ESN d'un USG peut-il être activé sur un
   autre USG du même modèle ?
2. Que signifie le message `The ESN of the license file does not match with
   the device` et que faire ?
3. Citez les trois informations minimales pour générer un fichier de licence
   sur le portail.
4. Un ticket S1 est ouvert à 16h30 avec un contrat Hi-Care Standard 9x5xNBD.
   Quand la pièce de remplacement arrivera-t-elle au plus tôt ?
5. Quelle est la différence entre NBD-S et NBD ?
6. En S1, que se passe-t-il si l'ingénieur TAC ne parvient pas à vous joindre
   dans l'heure ?
7. Pourquoi ne faut-il jamais activer une licence pendant un basculement
   actif/standby ?
8. Un AP361 eKit tombe en panne : quel est le bon premier réflexe côté support ?
9. Citez 4 des 10 informations obligatoires d'un ticket TAC.
10. Après un RMA d'USG6000, quelle vérification licence ne faut-il jamais oublier ?

## 122. Réponses du quiz

1. **Non.** Le `.dat` est cryptographiquement lié à un ESN précis ; sur un
   autre équipement, l'activation échoue (ESN mismatch). Il faut régénérer un
   fichier pour le nouvel ESN (transfert, section 32).
2. L'ESN saisi sur le portail ne correspond pas à l'équipement. Vérifier
   l'ESN réel (`display esn`, éventuellement après reboot), puis **régénérer
   un fichier avec le bon ESN** (section 30).
3. L'**Entitlement ID** (ou mot de passe d'activation de la preuve de droit),
   l'**ESN exact** de l'équipement cible, un **compte** sur le portail de
   licences (sections 18–20).
4. Le n° RMA généré après 15h00 un jour ouvré → enregistré le jour ouvré
   suivant → pièce reçue le **surlendemain ouvré** (section 85). (Si la panne
   est un jeudi avant un week-end prolongé : voir cas n°112.)
5. **NBD-S** : la pièce est *expédiée* le jour ouvré suivant (réception J+2/J+3).
   **NBD** : la pièce est *reçue* le jour ouvré suivant, si le RMA est généré
   avant 15h00 (section 85).
6. La sévérité est **temporairement abaissée** jusqu'au rétablissement du
   contact (section 64). D'où l'astreinte joignable 24x7 en S1.
7. Risque d'erreur *Failed to dispatch the license file* : le fichier peut
   échouer à se répliquer vers la MPU standby, surtout si son disque est
   plein ou si le basculement survient pendant l'activation (section 30).
8. Contacter le **partenaire** (circuit eKit : le partenaire est le point
   d'entrée unique, section 118), pas la hotline Huawei Enterprise directe.
9. Quatre parmi : modèle exact, version logicielle (VRP), ESN/S/N, n° de
   contrat de support, description du problème, impact métier, logs
   (diagnostic-information), topologie, contact joignable, actions déjà
   tentées (section 69).
10. Vérifier que la licence a été **transférée vers le nouvel ESN** :
    `display license` sur le nouvel équipement, fonctions + dates OK
    (sections 32 et 99, cas n°110).

## 123. Pour aller plus loin

**Documentation officielle (à consulter sur https://support.huawei.com) :**
- *HiSecEngine USG6000F License Usage Guide* — la référence licences des USG
  (activation, erreurs, dépannage).
- *Huawei Hi-Care Service Description* — paliers, SLA, responsabilités TAC/client.
- *Enterprise Product Warranty Policy* (par région) — durées et modalités
  par gamme de produits.
- *HiCare Support Services User Guide* — classification des sévérités S1–S4.
- Release Notes de votre version VRP exacte — prérequis et incompatibilités.

**Pratiques d'équipe recommandées :**
- Mettre en place le script nocturne `display license` + alertes J-90/J-60/J-30.
- Centraliser les syslogs de tout le parc (prérequis du support efficace).
- Maintenir un **spare froid** pour chaque équipement non redondé critique.
- Faire une **répétition annuelle** : un technicien ouvre un ticket « à blanc »
  (S4) pour vérifier que toute la chaîne (accès portail, collecte de logs,
  contacts) fonctionne.
- Formations : cursus HCIA/HCIP Datacom pour l'équipe (le vocabulaire TAC
  et les procédures y sont couverts).

**Autres guides de la série (même dossier) :**
- `onduleurs_ups_guide.md`, `proxmox_guide.md`, `huawei_*_guide.md`
  (AP361, AP761, S310, AR720, USG6000, eKit, eSight).

---

## Conclusion du guide

Licences, TAC et RMA ne sont pas des corvées administratives : ce sont les
**filets de sécurité** de votre production. Un chef de service qui tient son
registre de licences, son calendrier d'échéances et ses checklists RMA dort
mieux — et fait dormir son astreinte. Les pannes arriveront toujours ; la
différence entre une nuit blanche et un incident maîtrisé, c'est ce qui a été
**préparé avant**.

> *Dernière mise à jour : septembre 2026. Vérifiez les valeurs marquées
> « à vérifier sur le portail officiel » avant tout engagement.*

---

# L. ANNEXES OPÉRATIONNELLES

## 124. Script de contrôle automatique des licences (exemple)

Principe : chaque nuit, un script se connecte en SSH aux USG, lance
`display license`, extrait les dates d'expiration et alerte. Exemple de
logique (pseudo-code, à adapter à votre outillage — Ansible, Python/Netmiko,
ou simple expect) :

```text
POUR CHAQUE équipement_critique DANS inventaire:
    sortie = ssh(equipement, "display license")
    POUR CHAQUE ligne "Expire date : AAAA-MM-JJ" DANS sortie:
        jours_restants = date_expiration - aujourd_hui
        SI jours_restants <= 90: alerter("J-90", équipement, fonction)
        SI jours_restants <= 60: alerter("J-60", équipement, fonction)
        SI jours_restants <= 30: alerter("J-30 URGENT", équipement, fonction)
        SI jours_restants <= 0:  alerter("EXPIREE", équipement, fonction)
ENVOYER le rapport à la boîte d'alertes (section 117)
```

Exigences :
- Compte SSH **dédié** en lecture seule sur les équipements.
- La boîte destinataire doit être **surveillée** (pas une boîte générique).
- Tester le script après chaque changement de version VRP (le format de
  sortie peut évoluer).

## 125. Modèle de mail : demande de devis de renouvellement au partenaire

```text
Objet : Demande de devis — renouvellement licences UTM USG6000 (échéance JJ/MM/AAAA)

Bonjour [Nom],

Merci de nous chiffrer le renouvellement suivant :

- Équipement : USG6000 [modèle], site [nom], ESN [ESN]
- Licences : IPS + AV + URL Filtering (références actuelles : [part numbers])
- Durée souhaitée : 3 ans (merci de chiffrer aussi l'option 1 an pour comparaison)
- Date de début souhaitée : [JJ/MM/AAAA] (alignée sur la fin des droits actuels)

Merci de joindre à votre offre :
- les part numbers exacts et les dates de début/fin de droits,
- le délai de fourniture de la Proof of Entitlement après commande.

Échéance interne : devis attendu avant le [JJ/MM/AAAA] (J-60).

Cordialement,
[Nom] — [Fonction] — [Téléphone]
```

## 126. Modèle de mail : signalement au partenaire (circuit eKit)

```text
Objet : [eKit] AP361 site [nom] — panne suspectée, demande de prise en charge

Bonjour [Nom],

Nous rencontrons le problème suivant sur un équipement eKit :

- Modèle : [ex. AP361], S/N : [numéro], site : [nom/adresse]
- Symptôme : [description factuelle, méthode 5W]
- Depuis : [date/heure]
- Actions tentées : [liste]
- Pièces jointes : [diagnostic, photos, logs]

Merci de nous confirmer la prise en charge et le délai d'intervention
conformément à notre contrat de maintenance.

Joignable : [nom, téléphone direct].

Cordialement,
[Nom]
```

## 127. Fiche réflexe « panne à 2h du matin » (à imprimer)

```
┌─────────────────────────────────────────────────────────┐
│ PANNE RÉSEAU — FICHE RÉFLEXE (2h du matin)              │
├─────────────────────────────────────────────────────────┤
│ 1. RESPIRER. Ne rien rebooter en panique.               │
│ 2. Qualifier : quoi / où / depuis quand / qui impacté.  │
│ 3. Y a-t-il un contournement ? (lien secours, HA ?)     │
│    → OUI : l'activer, puis traiter en S2/S3.            │
│    → NON + site à terre : S1.                           │
│ 4. Collecter AVANT tout reboot :                        │
│    display diagnostic-information + display logbuffer    │
│ 5. Ouvrir le ticket (template section 70) ou appeler    │
│    la hotline avec le n° de contrat sous les yeux.      │
│ 6. Prévenir : [nom astreinte N+1 — À COMPLÉTER].         │
│ 7. Noter l'heure de chaque action (journal de bord).    │
│ 8. Ne pas fermer le ticket sans cause racine comprise.  │
├─────────────────────────────────────────────────────────┤
│ N° contrat Hi-Care : ............ [À COMPLÉTER]          │
│ Hotline TAC : ................... [À COMPLÉTER]          │
└─────────────────────────────────────────────────────────┘
```

## 128. Matrice de décision : réparer, RMA, ou acheter neuf ?

| Situation | Décision |
|---|---|
| Sous garantie / Hi-Care, panne matérielle confirmée | **RMA** (partie G) |
| Hors garantie, pièce disponible en spare, panne simple (alim, ventilo) | **Réparer** avec pièce d'origine via le partenaire |
| Hors garantie, carte mère / châssis, devis réparation > 60 % du neuf | **Acheter neuf** (et en profiter pour monter en gamme) |
| Équipement en fin de vie (EoS), panne matérielle | **Acheter neuf** : le RMA devient impossible à terme |
| DOA (défectueux à réception) | **Échange DOA** immédiat via le vendeur (section 106) |
| Dommage exclu de garantie (surtension, choc) | **Acheter neuf** + corriger la cause (parafoudre...) |

## 129. Gérer un parc multi-sites : organisation des dossiers

Arborescence recommandée (sur le partage d'équipe + sauvegarde) :

```text
parc_reseau/
├── site_paris/
│   ├── inventaire.md            (équipements, S/N, ESN, versions)
│   ├── licences/
│   │   ├── USG6000_21023..._IPS.dat
│   │   └── registre_licences.csv (section 39)
│   ├── contrats/
│   │   ├── hicare_2026-2029.pdf
│   │   └── preuves_de_droit/
│   ├── configs/
│   │   └── sauvegardes_AAAAMMJJ/
│   └── tickets/
│       └── SR-XXXX_2026-09-27/
├── site_lyon/
│   └── ... (même structure)
└── _transverse/
    ├── calendrier_echeances.csv (section 34)
    ├── contacts.md (section 117)
    └── procedures/ (ce guide + fiches réflexes)
```

## 130. Sauvegarde des configurations : politique minimale

Sans sauvegarde, un RMA devient une **reconstruction** (erreurs, oublis,
heures perdues). Politique minimale :

- [ ] Sauvegarde **automatique hebdomadaire** de tous les équipements
      (TFTP/SFTP vers un serveur + copie hors site).
- [ ] Sauvegarde **manuelle avant/après chaque changement** de configuration.
- [ ] Nommage : `config_<équipement>_AAAAMMJJ-HHMM.cfg`.
- [ ] Test de restauration **une fois par an** sur un équipement de test ou
      un spare froid.
- [ ] Les sauvegardes contiennent des secrets (mots de passe, clés) :
      stockage **chiffré**, accès restreint.

## 131. Bulletins de sécurité : s'abonner et réagir

1. Sur le portail support, abonnez-vous aux **bulletins de sécurité**
   pour vos gammes (USG, S310, AR...).
2. À chaque bulletin critique : évaluer l'exposition de votre parc
   (versions concernées ? fonction vulnérable activée ?).
3. Vérifier **immédiatement** vos droits de téléchargement du patch
   (cas vécu n°105).
4. Planifier le patch : d'abord en maquette / sur un site pilote, puis
   généralisation avec fenêtre de maintenance.
5. Tracer : bulletin → décision → patch appliqué (dossier d'audit, section 138).

## 132. Fin de vie (EoS/EoL) : anticiper, pas subir

- **EoS (End of Sale)** : on ne peut plus acheter l'équipement → les
  extensions de parc doivent changer de modèle.
- **EoL (End of Life)** : fin du support logiciel, puis matériel → tout
  RMA devient impossible à terme.
- Surveillez les annonces de fin de vie sur le portail pour vos modèles.
- Règle de pilotage : **ne jamais démarrer un nouveau site** sur un modèle
  annoncé en EoS ; planifier le renouvellement des modèles en EoL à 3 ans.

## 133. Qui paie quoi : imputation interne des coûts

Pour éviter les débats budgétaires en pleine urgence, faites valider en
amont :

| Coût | Imputation suggérée |
|---|---|
| Renouvellements de licences UTM | Budget récurrent du service (OPEX) |
| Contrats Hi-Care | Budget récurrent du service (OPEX) |
| RMA sous contrat | Coût nul (couvert) — tracer le temps passé |
| Achat suite à dommage exclu | Budget exceptionnel + plan d'action correctif |
| Spares froids | Investissement du service (amorti sur les incidents évités) |
| Formation de l'équipe | Budget formation |

## 134. Retour d'expérience : le débrief post-incident (template)

À faire sous 5 jours ouvrés après tout incident S1/S2 :

```text
REX INCIDENT — [titre] — [date]
1. Chronologie factuelle (heures + actions) :
2. Cause racine (technique) :
3. Cause racine (organisationnelle) : [ex. alerte non vue, doc manquante]
4. Ce qui a bien fonctionné :
5. Ce qui a mal fonctionné :
6. Actions correctives (qui / quoi / échéance) :
   - [ ] ...
   - [ ] ...
7. Faut-il mettre à jour ce guide / les checklists ? (oui/non + quoi)
Participants : [...]
```

Un REX sans action corrective datée et nommée est une réunion pour rien.

## 135. Former l'équipe : plan de montée en compétence administrative

| Public | Contenu | Format |
|---|---|---|
| Tous les techniciens | Fiche réflexe 2h du matin (127), collecte de logs (76–78), ouverture d'un ticket (70) | 1h d'atelier + exercice « à blanc » |
| Référent licences | Parties B–C, registre, script de contrôle (124) | Tutorat + ce guide |
| Chef de service / adjoint | Parties D–G, négociation contrats, REX (134), budget | Ce guide + revue annuelle |
| Nouvel arrivant | Lecture des parties A–B + visite du dossier parc (129) | Kit d'accueil, semaine 1 |

## 136. Lexique anglais-français : phrases types pour le TAC

| Situation | Phrase type (anglais) |
|---|---|
| Ouverture | "We are experiencing [symptom] on [model] since [time]." |
| Sévérité | "Business impact: [X users] affected, no workaround in place. Requesting S[S1/S2/S3/S4]." |
| Envoi de logs | "Please find attached the diagnostic-information output collected during the issue." |
| Demande de contournement | "Could you please provide a workaround while root cause analysis is ongoing?" |
| Escalade | "No progress since [date]. Requesting senior engineer review." (section 75) |
| RMA | "Hardware failure confirmed. Please proceed with RMA under contract [number]." |
| Licence temporaire | "Is it possible to issue a temporary license to cover the gap until [date]?" |
| Clôture | "Issue resolved since [date/time], stable for [X] hours. Ticket can be closed." |

## 137. Les 10 erreurs les plus fréquentes dans les tickets

1. Pas de version VRP exacte (« la dernière » ne veut rien dire).
2. Pas d'ESN/S/N → le TAC ne peut pas vérifier les droits.
3. Logs collectés **après** le reboot qui a tout effacé.
4. Captures d'écran au lieu de texte copié-collé (non cherchable).
5. Sévérité gonflée « pour aller plus vite » (section 108).
6. Ticket ouvert par email sans n° de contrat → 24h de perdues.
7. Deux tickets en parallèle pour le même problème (canaux différents).
8. Description = interprétation (« le DHCP est cassé ») au lieu du symptôme.
9. Contact injoignable (téléphone du bureau un dimanche).
10. Ticket fermé dès le contournement, sans cause racine → récidive.

## 138. Le dossier d'audit : ce que l'auditeur demandera

Un auditeur (sécurité, qualité, financier) vérifiera typiquement :

- [ ] L'inventaire des équipements (S/N, modèles, versions).
- [ ] Les licences : droits valides pour les fonctions utilisées ?
- [ ] Les contrats de support : couverture réelle vs criticité ?
- [ ] Les tickets S1/S2 de l'année : délais respectés ? REX faits ?
- [ ] Les RMA : traçabilité complète (n° RMA, retours) ?
- [ ] Les sauvegardes de configuration : existent-elles ? testées ?
- [ ] Les patchs de sécurité : appliqués dans quels délais ?
- [ ] Les accès au portail : nominatifs ? revus après les départs ?

Si chaque section de ce guide est appliquée, ce dossier **existe déjà**.

## 139. Plan de continuité : le classeur « si je suis absent »

Le chef de service part en congés : son adjoint doit pouvoir gérer une
urgence administrative. Le classeur (physique ou partagé) contient :

1. Ce guide (imprimé ou PDF à jour).
2. Les contacts (section 117) **vérifiés il y a moins de 3 mois**.
3. Les n° de contrats Hi-Care et les échéances (calendrier section 34).
4. Les accès : où sont les comptes portail (pas les mots de passe : la
   procédure de réinitialisation), qui a les droits.
5. Le registre des licences + l'emplacement des `.dat`.
6. La fiche réflexe 2h du matin (section 127).
7. La procédure RMA résumée en une page (partie G).

Testez-le : une fois par an, l'adjoint gère **seul** un exercice.

## 140. Index des commandes CLI citées dans le guide

| Commande | Usage | Section |
|---|---|---|
| `display esn` | Récupérer l'ESN pour les licences | 4, 16 |
| `display version` | Version VRP pour les tickets | 69 |
| `display license` | Vérifier fonctions et dates d'expiration | 26–27 |
| `display device` | S/N et état matériel | 69, 92 |
| `display diagnostic-information` | Pièce maîtresse pour le TAC | 76 |
| `display logbuffer` | Logs en mémoire (volatils) | 77 |
| `display logfile` | Logs persistants | 77 |
| `display current-configuration` | Config active (sauvegarde, diagnostic) | 76, 130 |
| `display environment` | Température, alimentations, ventilateurs | 92 |
| `license active <fichier>` | Activer un fichier de licence | 24 |
| `dir` | Vérifier la présence du `.dat` en flash | 23 |
| `tftp <srv> get <fichier>` | Transférer le `.dat` | 23 |

*(La syntaxe exacte peut varier selon les modèles et versions VRP —
toujours confirmer avec la documentation de votre version.)*

## 141. FAQ express — les questions qu'on pose tout le temps

**Q1. J'ai perdu le fichier `.dat`, que faire ?**
R : S'il a déjà été activé, l'équipement continue de fonctionner (la licence
survit au reboot). Régénérez un fichier depuis le portail avec le même ESN
si vous en avez besoin (réinstallation, RMA). Archivez-le cette fois (section 22).

**Q2. Puis-je utiliser le même `.dat` sur deux équipements identiques ?**
R : Non. Un fichier = un ESN. Il faut un fichier par équipement (section 31).

**Q3. La licence a expiré hier, tout est-il coupé ?**
R : Les fonctions sous licence se désactivent (ou cessent de se mettre à jour)
selon le produit ; le reste continue. Renouvelez et activez au plus vite
(sections 33, 36).

**Q4. Combien de temps pour un transfert de licence après RMA ?**
R : Comptez 24 à 72h ouvrées via le partenaire. Anticipez dès la réception
de la pièce de remplacement (section 32).

**Q5. Le TAC est-il vraiment joignable 24x7 ?**
R : Oui pour l'assistance technique (canaux hotline/email/portail), pour les
clients couverts. Les délais de *réponse* dépendent de la sévérité (section 63).

**Q6. J'ai un contrat Basic : aurai-je une pièce en 4h ?**
R : Non. Le 4h est réservé au palier Premier (P1/P2). En Basic, la pièce est
*expédiée* le jour ouvré suivant (section 83).

**Q7. Mon partenaire eKit ne répond pas, puis-je appeler Huawei ?**
R : Vous pouvez essayer, mais le circuit officiel eKit passe par le
partenaire. Si le vôtre est défaillant, c'est un problème **contractuel** à
traiter (mise en demeure, changement de partenaire), pas un problème TAC
(sections 111, 118).

**Q8. Faut-il une licence par AP361/AP761 ?**
R : En général non : c'est le contrôleur (ou la plateforme de gestion) qui
porte la licence de capacité « nombre d'AP » (section 12).

**Q9. Que faire si l'étiquette S/N est illisible ?**
R : Utilisez le portail (équipements enregistrés, section 49), le bon de
livraison d'origine, ou `display` en CLI si l'équipement démarre (cas n°104).

**Q10. Puis-je renvoyer le défectueux avant de recevoir le remplacement ?**
R : En Advance Replacement, non : gardez-le jusqu'au test du remplacement
(sections 97–98). En RFR, c'est le principe même (renvoi d'abord).

**Q11. Un firmware est-il couvert par la garantie standard ?**
R : Les mises à jour logicielles sont incluses ~1 an en garantie standard ;
au-delà, il faut un contrat logiciel actif (sections 51, 82).

**Q12. Comment prouver à ma direction qu'il faut du Hi-Care Premier ?**
R : Chiffrez le coût d'une indisponibilité (cas n°112 : 4 jours sans réseau)
vs le surcoût annuel du palier. Un seul incident évité rentabilise souvent
le contrat (sections 84–85).

## 142. Registre des incidents : modèle de suivi annuel

Tenez un registre unique de tous les incidents significatifs (S1/S2 et RMA).
Il sert au REX, à l'audit et à la négociation des contrats.

```csv
date;ticket;severite;equipement;site;symptome;cause_racine;duree_indispo;rma_numero;cout_estime;rex_fait;actions
2026-09-27;SR-2026-0912;S2;USG6000;Paris;Licences UTM expirees;Renouvellement non suivi;48h;non;1500 EUR;oui;Script J-90 mis en place
2026-10-03;SR-2026-1044;S1;S310;Lyon;Switch ne boote plus;Panne alimentation;26h;RMA-88412;800 EUR;oui;Spare froid achete
```

*(Valeurs fictives : exemple de format uniquement.)*

Colonnes minimales : date, n° ticket, sévérité, équipement, symptôme, cause
racine, durée d'indisponibilité, n° RMA le cas échéant, REX fait (oui/non).

## 143. Antisèche : les 20 points à retenir (une page)

1. La licence **autorise**, le support **assiste**, la garantie **remplace**.
2. Un `.dat` = **un ESN** = un équipement. Usage unique.
3. Toujours copier l'ESN depuis `display esn`, jamais le retaper.
4. Exiger la **Proof of Entitlement** à la commande, pas après.
5. Les licences UTM des USG sont des **abonnements** : calendrier J-90/J-60/J-30.
6. En HA : **une licence par membre**, mêmes fonctions, mêmes dates.
7. Archiver chaque `.dat` + tenir le **registre de licences**.
8. Ne jamais activer pendant un basculement actif/standby.
9. S1 = 30 min, S2 = 60 min, S3 = 2h, S4 = NBD (réponse TAC).
10. S1 seulement si l'activité est **réellement** bloquée ; rester joignable 24x7.
11. Ticket = modèle + version + ESN/S/N + contrat + 5W + logs + contact.
12. `display diagnostic-information` **pendant** le problème, avant tout reboot.
13. Le logbuffer est **volatil** : l'exporter avant de rebooter.
14. NBD-S = pièce *expédiée* J+1 ; NBD = pièce *reçue* J+1 (si RMA avant 15h).
15. RMA = **n° RMA** d'abord ; les délais courent à partir de sa génération.
16. Tester le remplacement **avant** de renvoyer le défectueux.
17. Après RMA : restaurer la config **et** transférer la licence (nouvel ESN).
18. eKit = **partenaire d'abord**, avec des SLA écrits dans le contrat.
19. Un spare froid sur étagère vaut mieux qu'un NBD un jour férié.
20. Ce qui n'est pas écrit n'existe pas : registre, calendrier, REX, dossier parc.

---

*Fin du guide. Bon courage — et que vos licences n'expirent jamais un vendredi soir.*
