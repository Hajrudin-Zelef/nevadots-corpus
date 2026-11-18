# STOCKAGE DATACENTER — Serveurs, HBA, SAS, NVMe, JBOD, Ceph : présent et futur

**Guide technique — rédigé en français — vérifications web au 27/09/2026**
**Public :** Zelef, chef de service systèmes & énergies. Ce guide parle chiffres, BOM,
dimensionnement électrique et refroidissement. Les prix sont des ordres de grandeur
issus de sources publiques datées ; ils bougent vite (allocation tendue en sept. 2026).
**Convention de marquage :**
- « vérifié le 27/09/2026 » = source web consultée ce jour (fiche produit, doc constructeur, presse).
- « à vérifier » = ordre de grandeur usuel du métier, à valider sur devis / datasheet exacte.
- « non trouvé au 27/09/2026 » = recherché, introuvable ; à surveiller.

> **Lien avec tes autres guides :** ce document est le socle matériel de `proxmox_guide.md`
> (Ceph, ZFS) et de `onduleurs_ups_guide.md` (dimensionnement électrique, NUT).
> Le chapitre Énergie (sections 183-202) est écrit pour ton métier d'énergéticien.

---

## 1. Objectif du guide

Concevoir, acheter, câbler, exploiter et faire évoluer un stockage datacenter moderne :
serveurs de stockage, cartes HBA, SAS, NVMe (U.2/U.3/EDSFF), JBOD, RAID vs erasure coding,
Ceph full NVMe et hybride, alternatives SDS, fiabilité, énergie, et trajectoire 2026→2030.

## 2. Ce que ce guide n'est pas

Ce n'est pas un catalogue : les références produits changent. C'est une méthode de
dimensionnement + des repères vérifiés au 27/09/2026 + une matrice de décision.
Chaque fois qu'un chiffre peut faire basculer un achat, il est sourcé ou marqué « à vérifier ».

## 3. Architecture moderne du stockage (le paysage en 2026)

```
 ┌─────────────┐     ┌──────────────┐     ┌────────────────┐
 │  DAS        │     │  NAS         │     │  SAN / SDS     │
 │ Direct-     │     │ File         │     │ Block / Object │
 │ Attached    │     │ NFS / SMB    │     │ iSCSI, FC,     │
 │ HBA+JBOD    │     │              │     │ NVMe-oF, Ceph  │
 └─────────────┘     └──────────────┘     └────────────────┘
        \                    |                     /
         \                   |                    /
          └──────────┬───────┴───────┬───────────┘
                     │  NVMe + 100G  │
                     │  EDSFF + CXL  │  ← 2026 : le socle change
                     └───────────────┘
```

Le mouvement de fond : le SAS recule vers le capacitaire (HDD), le NVMe prend le
performant, l'EDSFF remplace l'U.2 dans le dense, et le logiciel (Ceph, SDS) remplace
de plus en plus le RAID matériel.

## 4. Les trois contrats de stockage

| Contrat | Unité | Opération typique | Exemples |
|---|---|---|---|
| Bloc | secteur 512 B / 4 KiB | réécriture in place | RBD, iSCSI, LVM, VM |
| Fichier | plage d'octets dans un chemin | POSIX, verrous, rename atomique | NFS, SMB, CephFS, Lustre |
| Objet | objet entier (immuable) | PUT/GET/DELETE, S3 | RGW, MinIO, Swift |

Règle d'or : **le contrat choisi conditionne tout le reste** (latence, cohérence,
scalabilité). On ne met pas des VM sur de l'objet pur, on ne met pas de l'archivage
froid sur du bloc répliqué 3x.

## 5. Les quatre métriques qui comptent

| Métrique | Unité | Ce qu'elle mesure | Piège classique |
|---|---|---|---|
| IOPS | op/s | transactions | gonflés à QD élevé, irréalistes |
| Débit | Go/s | bande passante | séquentiel ≠ aléatoire |
| Latence | µs / ms | temps de réponse | p99, pas la moyenne |
| Profondeur de file | QD | parallélisme | QD1 (client) vs QD32+ (serveur) |

Pour Ceph et les bases : **latence p99 + IOPS à QD réaliste** > débit séquentiel marketing.
Un SSD NVMe Gen5 fait ~14 500 Mo/s en séquentiel (vérifié : Solidigm D7-PS1010,
KIOXIA CD9P-R, 27/09/2026) mais ce chiffre ne dit rien de ton workload.

## 6. To vs TiB : la confusion qui coûte 9 %

1 To (décimal) = 1 000 Go. 1 Tio (binaire) = 1 099,5 Go. Un « 7,68 To » constructeur =
7,68 × 10¹² octets = **6,99 Tio** utiles bruts avant formatage et overhead.
Sur 1 Po annoncé, l'écart To/TiB mange ~9 %. **Toujours convertir en Tio avant de
dimensionner**, et toujours déduire : over-provisioning (~7-14 %), filesystem (~2-5 %),
métadonnées Ceph/RAID.

## 7. Patterns de charge (workloads)

| Pattern | Lecture/écriture | Taille IO | Sensible à |
|---|---|---|---|
| VM / bases OLTP | 70/30, aléatoire | 4-16 KiB | latence, IOPS |
| Backup / archivage | séquentiel écriture | 128 KiB - 1 MiB | débit, €/To |
| Objet S3 / RGW | gros objets, WORM | 1-64 MiB | débit, capacité |
| IA / training | lecture séquentielle massive | gros blocs, QD très élevé | débit agrégé, GPU-feed |
| VDI | pics matinaux, aléatoire | 4-8 KiB | IOPS burst |

Depuis 2024-2025, les pipelines IA pilotés par GPU poussent des profondeurs de file
de plusieurs milliers (QD4096 en tests — vérifié : méthodologie TweakTown 2026).
Si ton stockage doit nourrir des GPU, dimensionne le **débit agrégé**, pas les IOPS unitaires.

## 8. Échelles : du lab au pétaoctet

| Échelle | Capacité utile | Architecture typique | Budget ordre de grandeur |
|---|---|---|---|
| Lab / PME | 10-50 To | 1-3 nœuds, ZFS ou Ceph HCI | à vérifier (devis) |
| Datacenter moyen | 100 To - 1 Po | 4-12 nœuds Ceph, EC ou réplica 3 | à vérifier |
| Cloud / IA | 1-50 Po | Ceph multi-sites, EDSFF, 400G | à vérifier |

Ce guide détaille le dimensionnement du cas « 100 To utiles » (sections 115-119)
et donne les formules pour extrapoler.

## 9. Méthode de lecture des tableaux « vérifié »

Quand tu lis « vérifié le 27/09/2026 », la valeur vient d'une fiche produit, d'un
communiqué ou d'une doc consultée ce jour-là. Le marché NAND/DRAM est en allocation
tendue en septembre 2026 (source : Fusion Worldwide Greensheet, sept. 2026 — vérifié) :
**les prix bougent, les délais s'allongent** sur SSD entreprise PCIe 5.0, HBM et DDR5
haute densité. Demande toujours un devis daté.

## 10. Plan du guide

Serveurs (11-26) → HBA (27-42) → SAS (43-58) → U.2/U.3/EDSFF (59-72) →
JBOD (73-88) → RAID vs EC vs JBOD (89-104) → Ceph full NVMe (105-132) →
Ceph hybride (133-148) → Alternatives SDS (149-164) → Fiabilité (165-182) →
Énergie (183-202) → À venir (203-216) → Pièges terrain (217-233) →
Glossaire (234) → Quiz (235-236).

---

# PARTIE B — SERVEURS DE STOCKAGE

## 11. Formats de châssis : 1U, 2U, 4U

| Format | Usage stockage typique | Contrainte |
|---|---|---|
| 1U | compute dense, EDSFF E1.S (16 baies) | refroidissement, peu de PCIe |
| 2U | standard : 12 à 32 baies NVMe/SAS | le meilleur compromis |
| 4U | capacitaire : 60 baies 3,5" | poids, profondeur, puissance |

En 2026, le 1U EDSFF monte à **un demi-pétaoctet en 1U16 E3.S** et **1 Po en 2U32 E3.S**
(vérifié : Supermicro, communiqué 2023 toujours d'actualité catalogue 2026).

## 12. Topologie 2U 12 baies

Le classique « 2U 12 baies 3,5" » : 12 disques en façade, carte mère + 2 CPU,
1-2 HBA internes. Idéal pour nœud Ceph HDD, backup, ou tête NVMe U.2 (12 baies).
Bande passante : 1 HBA 12G x8 = ~9,6 Go/s théoriques partagés — largement assez
pour 12 HDD (~2,4 Go/s agrégés).

## 13. Topologie 2U 24 baies

2U 24 baies 2,5" : le format SDS universel (Ceph, vSAN, HCI). 24 SSD SATA/SAS ou
NVMe U.2 selon backplane. Vérifie le **backplane** : simple expander 12G ou
tri-mode NVMe direct — ça change tout le câblage et le débit par baie.

## 14. Topologie 4U 60 baies (top-load)

Le « 4U 60 baies » top-load (chargement par le dessus) : 60 disques 3,5" SAS/SATA,
2 backplanes expander, 2+2 alimentations. Exemple vérifié : **Supermicro
SSG-6049P-E1CR60H** — 4U, 60 baies 3,5" SAS3/SATA3 hot-swap + 2 baies 2,5" arrière,
alimentations 2000 W redondantes Titanium (vérifié le 27/09/2026, fiches revendeurs).
Poids à plein : **~50-60 kg** — prévoir 2 personnes + rails lourds.

## 15. Topologie 4U 90+ baies

Des châssis 4U à 90-102 baies 3,5" existent chez les hyperscalers et certains OEM
(architectures « top-load double rangée »). Référence précise **non trouvée au
27/09/2026** dans les catalogues publics généralistes consultés → **à vérifier**
auprès des intégrateurs. Retiens : au-delà de 60 baies, le goulot devient
l'expander et la puissance (60 HDD × 10 W = 600 W rien que pour les disques).

## 16. Critère de choix n°1 : le backplane

| Type de backplane | Débit par baie | Usage |
|---|---|---|
| Direct (pas d'expander) | plein débit HBA | NVMe, perf maximale |
| Expander SAS 12G simple | partagé | HDD/SSD SATA, capacitaire |
| Tri-mode (SAS/SATA/NVMe) | négocié par baie | mixte, évolutif |
| EDSFF E3.S direct PCIe | Gen5 x4 par baie | dense NVMe 2026 |

Un backplane expander 12G x4 vers le HBA = 4,8 Go/s pour 24 disques : OK pour HDD,
**catastrophique pour 24 NVMe Gen4** (qui demandent ~7 Go/s chacun). Le backplane
se choisit **après** le workload, jamais avant.

## 17. Critère : profondeur, rails, poids

Un 4U 60 baies fait ~70-80 cm de profondeur (vérifié : 767 mm pour le SSG-6049P).
Vérifie : profondeur utile du rack (1000 mm mini conseillé), rails supportant
60+ kg, dégagement arrière pour câbles + PDU. Un châssis plein qui ne sort pas
de ses rails = maintenance impossible sans arrêt.

## 18. Alimentations : redondance et rendement Titanium

| Niveau 80 PLUS | Rendement à 50 % de charge |
|---|---|
| Gold | 92 % |
| Platinum | 94 % |
| Titanium | 96 % |

Vérifié : le SSG-6049P-E1CR60H est livré en 2000 W redondants **Titanium**
(27/09/2026). Sur un nœud 60 baies qui tire ~900 W en continu, passer de Gold
à Titanium économise ~35-40 W en permanence, soit ~350 kWh/an — à multiplier
par le nombre de nœuds et le PUE. Pour ton métier : **impose Titanium sur tout
nœud de stockage neuf**.

## 19. Refroidissement : le flux d'air décide

Les disques 3,5" empilés en top-load créent des zones mortes. Règles :
ventilateurs hot-swap redondants (N+1), sondes par zone, et **ne jamais faire
fonctionner un 60 baies avec des baies vides sans obturateurs** : l'air prend
le chemin le plus facile et les disques restants chauffent. Température cible
disques : < 40 °C en régime établi (au-delà, l'AFR grimpe).

## 20. Référence vérifiée : Supermicro SSG-6049P-E1CR60H

Fiche consolidée (vérifié le 27/09/2026, plusieurs revendeurs) :
4U, 60 baies 3,5" hot-swap SAS3/SATA3 + 2 baies 2,5" arrière, double Xeon Scalable,
24 slots DDR4 (jusqu'à 6 To), 3 slots PCIe, alimentations 2000 W redondantes
Titanium, expander SAS3 12 Gb/s. Statut catalogue : **fin de vie (EOL)** chez
certains revendeurs — pour du neuf 2026, vise la génération suivante (W790/X13)
ou du reconditionné garanti. Prix : **à vérifier** (sur devis, configuration dépendante).

## 21. Références vérifiées : Supermicro EDSFF all-flash

Communiqué Supermicro (vérifié le 27/09/2026) :
- **SSG-121E-NE316R** : 1U, 16 baies E3.S
- **SSG-221E-NE324R** : 2U, 32 baies E3.S
- **SSG-121E-NES24R** : 1U, 24 baies E1.S
- Capacité : 0,5 Po en 1U16, **1 Po en 2U32** (avec SSD ~30 To)
- PCIe Gen5, architecture NUMA symétrique, 2 slots PCIe Gen5 x16 + 2 AIOM.
C'est la trajectoire « pétaoctet par U » : le format à viser pour tout projet
all-flash neuf à partir de 2026.

## 22. Références marché : Dell, HPE, Lenovo

Dell PowerEdge (série R, ex. R760xd), HPE ProLiant (DL380), Lenovo ThinkSystem
(SR650) : les trois généralistes proposent des 2U 12-24 baies et des 4U/5U
capacitaires. Modèles exacts et options backplane **à vérifier** au catalogue
du trimestre (les gammes tournent vite). En pratique : compare toujours à
iso-backplane et iso-alimentation, pas à iso-prix catalogue.

## 23. BOM exemple : nœud 60 baies HDD (capacitaire froid)

| Ligne | Qté | Ordre de grandeur |
|---|---|---|
| Châssis 4U 60 baies + 2×2000 W Titanium | 1 | à vérifier (devis) |
| Carte mère double socket + 2 CPU 16c | 1 | à vérifier |
| RAM 128 Go ECC DDR4/DDR5 | 1 lot | à vérifier |
| HBA 12G 16 ports externes (ex. 9500-16e) | 1 | ~300-500 € (ordre de grandeur public) |
| HDD 20-30 To CMR 7200 tr/min | 60 | ~15-20 €/To → 18 000-36 000 € |
| SSD boot 2,5" (miroir) | 2 | à vérifier |
| Rails lourds + obturateurs | 1 lot | à vérifier |

Capacité brute : 60 × 20 To = 1,2 Po (1,09 Pio). Avec EC 8+3 : ~0,87 Po utiles.

## 24. BOM exemple : nœud NVMe E3.S (performant)

| Ligne | Qté | Ordre de grandeur |
|---|---|---|
| Châssis 2U 32 baies E3.S Gen5 | 1 | à vérifier |
| 2 CPU + 512 Go DDR5 | 1 lot | à vérifier |
| 2× NIC 100 GbE | 2 | à vérifier |
| SSD E3.S 7,68 To Gen5 (ex. CD9P-R, D7-PS1010) | 32 | ~350-600 €/To → 86 000-147 000 € |
| 2×1600 W Titanium | inclus | — |

Capacité brute : 32 × 7,68 To = 245,8 To. Réplica 3 → ~82 To utiles par nœud.

## 25. Calcul d'air simplifié (règle terrain)

1 W à évacuer ≈ 1,7 m³/h d'air pour ΔT = 2 °C (ordre de grandeur, à vérifier
selon hygrométrie). Un nœud 60 baies à 900 W demande donc ~1 500 m³/h.
Vérifie que la rangée et les CRAC suivent **avant** d'empiler 10 nœuds
(9 kW, 15 000 m³/h) dans une rangée prévue pour du compute léger.

## 26. Montage rack : sécurité

60 baies pleines ≈ 55-65 kg. Règles : rails **dédiés lourds** (pas les rails
fins du 1U), 2 personnes minimum, chariot élévateur de rack si disponible,
disques installés **après** mise en rack quand c'est possible. Un 4U qui bascule
d'un rack, c'est un accident du travail + 60 disques morts.

---

# PARTIE C — CARTES HBA

## 27. Principe d'une HBA

Une HBA (Host Bus Adapter) expose les disques **tels quels** au système
d'exploitation : pas de RAID, pas de cache, pas d'abstraction. C'est le mode
« dumb pipe » intelligent : le logiciel (ZFS, Ceph, mdadm) voit chaque disque,
son SMART, ses erreurs. Pour tout SDS moderne : **HBA obligatoire, RAID matériel
interdit sur les disques de données**.

## 28. IT mode vs IR mode

| Mode | Nom Broadcom | Comportement |
|---|---|---|
| IT (Initiator-Target) | HBA / « eHBA » | JBOD pur, chaque disque visible |
| IR (Integrated RAID) | MegaRAID | RAID 0/1/5/6/10/50/60 matériel |

Le même silicium peut souvent basculer d'un firmware à l'autre (ex. 9400/9500 :
firmware HBA vs MegaRAID). **Vérifie le firmware livré** : un « 9500-16i » peut
arriver en IR si mal commandé. Pour Ceph/ZFS : IT ou rien.

## 29. Pourquoi IT mode pour ZFS et Ceph

ZFS et Ceph gèrent eux-mêmes redondance, checksums, scrub et rebuild. Un RAID
matériel entre les deux :
- masque les erreurs disque (le contrôleur « corrige » en silence),
- casse l'alignement des rebuilds (rebuild RAID 6 de 60×20 To = semaines),
- ajoute un point de panne (cache, batterie/supercondensateur).
Exception : les **disques système** (boot) peuvent rester en RAID 1 matériel.

## 30. Broadcom HBA 9400-16i (génération 12G)

La 9400-16i (SAS3408/SAS3416, PCIe 3.0, 12G SAS) est la HBA 12G la plus répandue
des générations précédentes. En 2026 elle reste pertinente pour du HDD/SATA
capacitaire d'occasion/reconditionné. Prix public : **à vérifier** (marché gris
actif). Pour du neuf NVMe/Gen4 : vise la 9500.

## 31. Broadcom HBA 9500-8i / 9500-16i / 9500-8e / 9500-16e (vérifié)

Fiche consolidée (vérifié le 27/09/2026, fiches revendeurs) :
- **Tri-mode** : 12 Gb/s SAS, 6 Gb/s SATA, PCIe 4.0 (NVMe)
- Hôte : PCIe 4.0 x8 (16 GT/s par voie)
- Connecteurs : SFF-8654 SlimSAS (internes : 1× pour 8i, 2× pour 16i ;
  externes : SFF-8644 pour les versions « e »)
- Capacité : **1024 périphériques SAS/SATA, 32 NVMe**
- Débit : jusqu'à 13 700 Mo/s (séquentiel 256K), 3 M IOPS (4K lecture aléatoire)
- MTBF : 5 000 000 h à 40 °C ; garantie 3 ans
- Conso : **6,12 W (8e), 8,74 W (16e)** — vérifié, utile au bilan énergie
- Outils : LSI Storage Authority, storcli, HII (UEFI)
- UBM ready (SFF-TA-1005) pour baies U.3
- Prix public constaté : **à vérifier** (varie ~250-450 € selon version)

## 32. Broadcom 9600-16e / MegaRAID 9670 (24G, vérifié)

Fiche consolidée (vérifié le 27/09/2026) :
- **24 Gb/s SAS** (SAS-4), SATA 6G, PCIe 4.0 NVMe (tri-mode)
- Hôte : PCIe 4.0 x16 (9670W-16i) ou x8 (9670-24i)
- Connecteurs : **SFF-8654 SlimSAS 24G** (interne), SFF-8674 (externe)
- Débits SAS négociés : 22,5 / 12 / 6 Gb/s par voie (le 24G effectif ≈ 22,5G
  après encodage — détail vérifié sur fiche 9600)
- Devices : 240 SAS/SATA, 32 NVMe ; cache 8 Go + CacheVault CVPM05 (versions RAID)
- RAID 0/1/5/6/10/50/60 + JBOD
- **DataBolt2** : permet du 24G vers le contrôleur avec des disques/backplanes
  12G/6G existants (vérifié)
- Disponibilité : en stock chez les revendeurs consultés le 27/09/2026.

## 33. Microchip Adaptec SmartHBA / SmartIOC 2200 (24G, vérifié)

Microchip a démontré la **première interopérabilité 24G SAS de bout en bout**
avec KIOXIA (communiqué vérifié, 2021, toujours la référence publique) :
SmartROC 3200 (RAID-on-Chip PCIe Gen4 tri-mode), **SmartIOC 2200** (contrôleur
HBA), expander **SXP 24G SAS**. En 2026, Microchip reste le second fournisseur
de silicium SAS avec Broadcom. Pour un appel d'offres : **toujours mettre
Broadcom et Microchip en concurrence**.

## 34. Quand HBA vs RAID hardware : matrice

| Cas | Choix |
|---|---|
| Ceph / ZFS / SDS | HBA IT, toujours |
| Boot / OS | RAID 1 matériel ou miroir logiciel |
| Serveur applicatif simple, pas de SDS | RAID hardware 1/10 |
| Base de données sur NVMe direct | pas de carte du tout (NVMe direct CPU) |
| JBOD externe capacitaire | HBA externe 16e |

## 35. Queue depth et file d'attente

Une HBA 9500 encaisse des profondeurs de file énormes (3 M IOPS annoncés),
mais le goulot se déplace : CPU (interruptions), puis réseau. En pratique
Linux : vérifie `nr_requests`, le scheduler (`none` pour NVMe), et
l'affinité IRQ. Un NVMe Gen5 à 14 Go/s sature un cœur moderne en traitement
d'interruptions — prévois **2+ cœurs par SSD NVMe très sollicité**.

## 36. Multipath et MPIO

En SAS double-domaine (dual-path) vers un JBOD, chaque disque apparaît deux fois :
il faut **multipath** (Linux `multipathd` / `mpath`) pour n'en voir qu'un.
Sans multipath : corruption assurée dès qu'un chemin flappe. Règle : dual-path
= multipath configuré **et testé** (débranche un câble en burn-in, vérifie).

## 37. Firmware : la discipline

- Note la version firmware HBA **et** expander au déballage.
- Ne mets à jour qu'avec le package du **constructeur du serveur** (pas le
  générique Broadcom) sauf consigne contraire — les OEM verrouillent parfois.
- Après flash : revérifie le mode IT/IR (certains flashs réinitialisent).
- Garde l'ancien firmware en rollback 48 h après toute MaJ en production.

## 38. Outils : storcli, LSA, sas3ircu

| Outil | Usage |
|---|---|
| `storcli` | CLI : état, firmware, locate, rebuild (vérifié : livré avec les 9500/9600) |
| LSI Storage Authority | GUI web de supervision |
| HII (UEFI) | config pré-boot |
| `sas3flash` / `storcli` | flash firmware |

Automatise : un script qui collecte `storcli /c0 show all` par nœud et l'envoie
à ta supervision (Zabbix — voir ton `zabbix_guide.md`). Une HBA qui disparaît
du PCIe = nœud à isoler immédiatement.

## 39. NVMe direct vs Tri-Mode

| Approche | Câblage | Cas |
|---|---|---|
| NVMe direct CPU (backplane PCIe) | MCIO / SlimSAS x4 par baie | perf max, pas de carte |
| Tri-Mode HBA | SFF-8654 vers backplane UBM | mixte SAS/SATA/NVMe, souplesse |

Le NVMe direct supprime la carte (moins de latence, moins de watts) mais fige
le châssis en « tout NVMe ». Le tri-mode garde la flexibilité. Pour Ceph full
NVMe neuf : **NVMe direct si le châssis le permet**, tri-mode sinon.

## 40. Arbre de décision HBA

```
Disques = HDD/SATA capacitaires ? ──oui──▶ HBA 12G (9500-16e / 9400 recond.)
          │
          non
          ▼
Disques = NVMe ? ──oui──▶ Backplane NVMe direct ? ──oui──▶ pas de HBA
          │                        │
          │                       non──▶ HBA tri-mode 9500-16i
          non
          ▼
Mixte / évolutif ? ──oui──▶ HBA tri-mode 9500/9600
```

## 41. eHBA : le mode hybride (9600)

Les 9600 existent en version **eHBA** : accélération RAID pour quelques volumes
+ présentation JBOD/HBA pour le reste. Utile en transition (boot RAID 1 + data
JBOD sur la même carte). À manier avec prudence : un mauvais profil et tes
disques Ceph se retrouvent derrière du RAID sans que tu le voies.

## 42. Pièges HBA (rappel, détail en section 217+)

1. Firmware IR livré au lieu d'IT. 2. Câble SFF-8643 branché sur port SFF-8654
sans adaptateur (ça ne rentre pas — forcer = broches pliées). 3. HBA x8 dans un
slot x4 électrique : la moitié du débit. 4. Pas de flux d'air sur la HBA :
les 9500/9600 chauffent, prévoir le déflecteur d'air du châssis.

---

# PARTIE D — SAS : GÉNÉRATIONS, CÂBLAGE, LIMITES

## 43. Générations SAS : 6G et 12G

| Génération | Norme | Débit par voie | Statut 2026 |
|---|---|---|---|
| SAS-2 | 6 Gb/s | 600 Mo/s | legacy, SATA-like |
| SAS-3 | 12 Gb/s | 1 200 Mo/s | **standard capacitaire actuel** |
| SAS-4 | 24 Gb/s (22,5 effectifs) | ~2 250 Mo/s | **disponible (vérifié)** |

Le 12G reste le standard des baies HDD/SSD SAS neuves en 2026 : mature,
pas cher, suffisant pour des HDD (250-300 Mo/s unitaires).

## 44. SAS-4 / 24G : état vérifié au 27/09/2026

Le 24G SAS **existe et se vend** : contrôleurs Broadcom série 9600/9670
(SFF-8654/SFF-8674) en stock chez les revendeurs, interopérabilité 24G
démontrée Microchip+KIOXIA, FEC (correction d'erreur) intégrée à la norme.
Mais : les **disques 24G** restent rares et chers, et le gain réel sur HDD
est nul (un HDD ne sature pas le 12G). Le 24G a du sens pour : SSD SAS
rapides, agrégation d'expanders, et préparation de l'avenir. Pour du HDD
pur en 2026 : **reste en 12G**.

## 45. Câblage SFF-8087 / 8088 (legacy)

Mini-SAS interne (8087) / externe (8088), génération 6G. Encore présents sur
le matériel d'occasion. Règle : ne pas mélanger avec du 12G+ sans vérifier
la qualité du câble — un vieux 8087 peut négocier en 6G seulement et
brider une chaîne 12G.

## 46. SFF-8643 / 8644 : Mini SAS HD (12G)

Le standard 12G : **SFF-8643** (interne, 4 voies) et **SFF-8644** (externe).
Un câble 8644 = 4 voies × 12G = 48 Gb/s = 4,8 Go/s. C'est ce qui relie une
HBA 9500-16e à un JBOD. Longueur cuivre passive : ~2-4 m selon qualité
(au-delà : câbles actifs ou optiques — à vérifier par référence).

## 47. SFF-8654 SlimSAS 24G (vérifié)

Le connecteur 24G : **SFF-8654** « SlimSAS », 8 voies (x8) en interne.
Vérifié sur les fiches 9500-16i (2× SFF-8654) et 9670-24i (3× SFF-8654).
Attention : le 8654 existe en versions 4 voies et 8 voies, et en brochage
SAS vs PCIe — **le câble doit matcher le brochage du backplane** (SFF-9402).
Un mauvais câble SlimSAS = lien qui ne monte pas ou négocie en PCIe
au lieu de SAS.

## 48. SFF-8674 externe (vérifié)

Le pendant externe du 24G, présent sur les 9600-16e (vérifié : fiches
produit). Remplace le 8644 pour les liaisons HBA → JBOD en 24G.
Compatibilité descendante : un JBOD 12G (8644) se branche sur un port
8674 **avec le bon câble adaptateur**, négociation à 12G.

## 49. Longueurs et limites physiques

| Type | Distance indicative | Note |
|---|---|---|
| Cuivre passif SAS HD | 1-4 m | dépend de l'AWG et du débit |
| Cuivre actif | jusqu'à ~10 m | à vérifier par référence |
| Optique AOC | 10-100 m | cher, pour baies distantes |

Règle terrain : **le plus court possible**, jamais de câble enroulé en
boucle (diaphonie), et un câble de rechange de chaque type en stock.
Un câble SAS qui vieillit = erreurs CRC intermittentes = enfers de diagnostic.

## 50. Expanders SAS : principe

Un expander est un « switch SAS » : 1 port amont vers la HBA, N ports vers
les disques. Il permet 60 disques derrière une HBA 16 voies. Coût : la
bande passante amont est **partagée** (voir section 80), et l'expander
ajoute ~quelques µs de latence. Deux expanders en cascade = bande passante
encore divisée.

## 51. DataBolt2 (vérifié le 27/09/2026)

Technologie Broadcom (série 9600) : le contrôleur parle **24G vers l'amont**
tout en servant des disques/backplanes **12G/6G** en aval, avec buffering
intelligent. Intérêt : upgrader la HBA sans changer les disques ni le
backplane, et gagner quand même sur l'agrégat. Ce n'est pas de la magie :
le débit d'un disque 12G reste 12G.

## 52. T-10 EEDP / DIF (vérifié)

End-to-End Data Protection : 8 octets de protection (guard tag, app tag,
ref tag) ajoutés à chaque bloc 512 B → blocs 520 B sur les disques
entreprise. Vérifié : supporté par les 9600. Avec ZFS/Ceph qui font leurs
propres checksums, l'EEDP est redondant mais inoffensif ; en RAID matériel
classique, il reste utile. Les disques « 512e » grand public ne l'ont pas.

## 53. SATA sur contrôleur SAS : les règles

Le SAS accepte le SATA (tunneling STP), l'inverse est faux. Mais :
- pas de double-domaine en SATA (un seul chemin),
- pas de commandes SCSI avancées (pas d'EEDP),
- les SATA derrière un expander SAS partagent la file plus brutalement,
- **SATA + expander + RAID = la combinaison la plus fragile** : privilégie
  le branchement direct ou des expanders de qualité.
Pour Ceph sur SATA : HBA IT + backplane correct = OK, c'est courant.

## 54. Power management SAS (vérifié)

La norme SAS 2.1 (supportée par les 9600, vérifié) inclut le power management
des liens. En pratique datacenter : on le laisse souvent désactivé sur les
liens HBA↔JBOD (la renégociation coûte de la latence), et on gère l'énergie
au niveau disque (spin-down, section 188) plutôt qu'au niveau lien.

## 55. Négociation de vitesse

Chaque lien négocie au plus haut débit commun : 24G↔24G = 22,5G effectifs,
24G↔12G = 12G, etc. (vérifié : 22,5/12/6 Gb/s sur les 9600). Diagnostic :
`storcli` / `sas2ircu` affichent la vitesse négociée par phy. **Un lien qui
négocie en dessous de son nominal = câble, connecteur ou firmware à
suspecter en premier.**

## 56. Compatibilité descendante

Le SAS est historiquement exemplaire : un contrôleur 24G parle à un disque
6G de 2012. C'est ce qui rend les migrations douces possibles (HBA neuve,
disques anciens). L'exception : certains très vieux expanders 3G/6G avec
des disques 12G+ peuvent causer des resets — à tester en burn-in.

## 57. Limites : nombre de périphériques

| Élément | Limite indicative |
|---|---|
| HBA 9500/9600 | 1024 SAS/SATA, 32 NVMe (vérifié) |
| Domaine SAS (adressage) | 16 384 adresses théoriques |
| Pratique par HBA | quelques centaines avant que le firmware ne rame |

En pratique, on ne met jamais 1000 disques sur une HBA : le goulot de
bande passante arrive bien avant (section 80).

## 58. Quand le SAS a encore du sens en 2026

- Capacitaire HDD : le 12G est le standard, pas cher, mature.
- SSD SAS 12G/24G : dual-port natif (deux chemins sans expander spécial),
  utile en SAN classique.
- Transition : DataBolt2 permet de monter en 24G sans tout changer.
- En revanche : pour du NVMe neuf performant, **le SAS est un détour** —
  vise le PCIe direct ou le tri-mode.

---

# PARTIE E — U.2 / U.3 / EDSFF (NVMe)

## 59. U.2 (SFF-8639) : le standard installé

Le 2,5" 15 mm avec connecteur SFF-8639 : PCIe x4 (jusqu'à Gen5), hot-swap,
présent dans 90 % des serveurs NVMe 2020-2025. Limites : épais, bloque
l'air en façade, 25 W max pratiques par baie sans refroidissement spécial.
Reste un excellent choix « sûr » en 2026 pour du NVMe Gen4.

## 60. U.3 : la différence avec U.2

U.3 = **même connecteur physique** que U.2, mais avec négociation **tri-mode**
(SAS/SATA/NVMe) sur les mêmes broches, pilotée par le backplane (UBM).
Un SSD U.3 fonctionne dans une baie U.3 ; dans une baie U.2 pure, un SSD
U.3 **peut ne pas être reconnu** (pas de négociation tri-mode côté hôte).
Règle d'achat : **baie U.3 + SSD U.3 = le couple sûr et évolutif**.

## 61. UBM / SFF-TA-1005 (vérifié)

Universal Bay Management : le standard qui permet au backplane de dire au
SSD quel protocole parler. Vérifié : les HBA Broadcom 9500 sont « UBM ready ».
Sans UBM, le tri-mode ne fonctionne pas : la baie doit être câblée et
firmwarée pour. À vérifier à la commande du châssis : « backplane UBM ? ».

## 62. Tableau de compatibilité U.2 / U.3

| SSD \ Baie | U.2 pure | U.3 (UBM) |
|---|---|---|
| SSD U.2 NVMe | OK | OK |
| SSD U.3 tri-mode | **non garanti** | OK |
| SSD SAS 12G | OK (baie SAS) | OK |

En cas de doute : mets tout en U.3 des deux côtés. Le surcoût est faible,
le regret d'une baie incompatible est grand.

## 63. EDSFF : pourquoi un nouveau format

L'U.2 n'a jamais été pensé pour la flash : boîtier épais, connecteur en bout,
air bloqué. L'EDSFF (Enterprise & Datacenter Standard Form Factor, SNIA)
redessine le SSD autour de la NAND : **règles fines verticales, air qui
circule entre elles, plus de surface de PCB, plus de puissance admissible**.
Résultat vérifié : 0,5 Po en 1U16 E3.S, 1 Po en 2U32 (Supermicro, 27/09/2026).

## 64. E1.S (vérifié)

Le format « 1U » : règle courte (~111 mm), épaisseurs 5,9 / 9,5 / 15 / 25 mm.
Le remplaçant désigné du M.2 en datacenter, hot-swap en façade. Vérifié :
Samsung PM9A3 (Gen4), KIOXIA NX1 (E1.S, refroidissement liquide direct —
27/09/2026), Micron 9650 (Gen6, E1.S + E3.S). Densité typique : 24 baies
E1.S en 1U.

## 65. E1.L (vérifié)

La « règle longue » (~318 mm) : maximise la surface NAND → **capacité par
baie**. Moins courant que E1.S/E3.S dans les catalogues généralistes 2026,
plutôt chez les hyperscalers. Si ton besoin est « max de To par U en 1U »,
c'est lui ; sinon E1.S suffit.

## 66. E3.S (vérifié)

Le format « 2U » généraliste : carte ~76 × 112,75 mm, 7,5 mm d'épaisseur,
**le successeur désigné de l'U.2**. Vérifié : KIOXIA CD9P-R 7,68 To E3.S
(14 800 Mo/s, 2 600K IOPS), Solidigm D7-PS1010 E3.S (1,92-15,36 To),
Samsung PM9D3 128 To E3.S en production, Micron 9650 E3.S Gen6. **Pour tout
projet NVMe neuf 2026+ : E3.S par défaut.**

## 67. E3.L (vérifié)

Version longue de E3 pour très haute capacité. Mentionné dans la
documentation EDSFF/SNIA ; moins déployé que E3.S dans l'offre publique
2026. À considérer si les roadmaps 245 To+ (section 208) arrivent en E3.L.

## 68. EDSFF et PCIe Gen5 / Gen6

L'EDSFF est agnostique de la génération PCIe : les baies E3.S actuelles
sont câblées Gen5 x4 (16 voies possibles sur E3). Le **Micron 9650**
(vérifié, août 2026) est le premier SSD **PCIe Gen6** E1.S/E3.S du marché,
décliné en versions Pro (lecture intensive) et Max (mixte), air ou liquide.
Gen6 = ~28 Go/s par x4 : le réseau (même 400G) devient le goulot avant le SSD.

## 69. Refroidissement EDSFF : air et liquide (vérifié)

Deux faits vérifiés le 27/09/2026 :
- **KIOXIA NX1** : premier E1.S avec **refroidissement liquide direct**,
  +38 % d'écriture séquentielle, conforme OCP Datacenter NVMe SSD 2.6, FDP.
- **Solidigm D7-PS1010 E1.S** : version **cold plate** (plaque froide)
  co-développée avec NVIDIA, états de puissance 5-25 W.
Message : à partir de Gen5, le SSD est un composant **thermique** autant
qu'électronique. Prévois le flux d'air (ou la boucle d'eau) **avant** la
densité.

## 70. Gammes SSD datacenter 2026 (vérifié)

| Modèle | Interface | Format | Capacités | Positionnement |
|---|---|---|---|---|
| Solidigm D7-PS1010 | Gen5 x4 | U.2 / E3.S / E1.S | 1,92-15,36 To | performance (vérifié) |
| KIOXIA CD9P-R | Gen5 x4 | E3.S | 1,92-30,72 To | lecture intensive, 1 DWPD (vérifié) |
| KIOXIA CD8P | Gen5 x4 | U.2 / E3.S | 1,6-12,8 To | mixte, 3 DWPD (vérifié) |
| Samsung PM9D3 | Gen5 x4 | E3.S | jusqu'à 128 To | datacenter (vérifié) |
| Solidigm D5-P5430 | Gen4 x4 | U.2 / E3.S | jusqu'à 30,72 To | QLC capacitaire, 0,58 DWPD (vérifié) |
| Micron 9650 | **Gen6** | E1.S / E3.S | à vérifier | premier Gen6 (vérifié) |

Prix : ~350-600 €/To constatés pour du NVMe entreprise PCIe 5.0
(ordre de grandeur public, allocation tendue — **à vérifier** sur devis).

## 71. Le marché NAND en septembre 2026 (vérifié)

Fusion Worldwide Greensheet (sept. 2026, vérifié) : **SSD entreprise
PCIe 5.0 / haute capacité = allocation contrainte, prix en hausse**,
comme HBM et DDR5 haute densité. Le NAND spot grand public reste plus
souple. Traduction achats : commandes fermes tôt, stocks tampons sur les
références critiques, et second sourcing (2 fournisseurs qualifiés).

## 72. Tableau comparatif des formats NVMe

| Format | Idéal pour | Densité | Thermique | Maturité 2026 |
|---|---|---|---|---|
| U.2 15 mm | existant, Gen4 | 24/2U | correcte | maximale |
| U.3 | mixte tri-mode | 24/2U | correcte | haute |
| E1.S | 1U dense, Gen5/6 | 24/1U | bonne (+liquide) | croissante |
| E3.S | 2U all-flash neuf | 32/2U | très bonne | **standard neuf** |
| E1.L/E3.L | capacité extrême | variable | à étudier | émergente |

---

# PARTIE F — JBOD : TOPOLOGIES, CÂBLAGE, LIMITES

## 73. Principe du JBOD

JBOD = « Just a Bunch Of Disks » : un châssis **sans carte mère ni CPU**,
juste des baies, des expanders, des alimentations et des ventilateurs.
Il se pilote depuis un serveur via une HBA externe. Usage : étendre un
nœud de stockage au-delà de ses baies internes, ou construire du capacitaire
piloté par un petit serveur.

## 74. JBOD vs serveur de stockage

| Critère | JBOD | Serveur de stockage |
|---|---|---|
| CPU/RAM | non (tête externe) | oui |
| €/baie | moins cher | plus cher |
| Flexibilité | dépend de la tête | autonome |
| Point de panne | tête unique | nœud complet |

Pour Ceph : un JBOD derrière un nœud Ceph, c'est 60 OSD sur **un seul**
domaine de panne (la tête). Mieux vaut souvent 2 nœuds de 30 baies que
1 nœud + 1 JBOD de 60.

## 75. Topologie simple : HBA → JBOD

```
 ┌──────────┐   SFF-8644/8674    ┌──────────────────────┐
 │ Serveur  │────────────────────▶│ JBOD 60 baies        │
 │ HBA 16e  │◀────────────────────│ Expander A           │
 └──────────┘   (2 câbles = 8     └──────────────────────┘
                 voies = 9,6 Go/s)
```

Deux câbles externes = 8 voies 12G = 9,6 Go/s théoriques pour 60 disques.
Pour 60 HDD (~15 Go/s si tous à fond — irréaliste) c'est OK ; pour des
SSD SATA (60 × 550 Mo/s = 33 Go/s), **c'est un goulot** : ajoute des
câbles ou passe en 24G.

## 76. Topologie cascade (daisy chain)

```
 Serveur (HBA 16e)
    │ 8 voies
    ▼
 ┌──────┐  4 voies ┌──────┐
 │JBOD-1│─────────▶│JBOD-2│
 │60    │◀─────────│60    │
 └──────┘          └──────┘
   120 disques sur 8 voies = 9,6 Go/s partagés
```

La cascade économise des ports HBA mais **divise la bande passante** et
ajoute un domaine de panne (JBOD-1 down = JBOD-2 isolé, sauf double
domaine). Limite raisonnable : 2 JBOD en cascade, jamais 3+ en production.

## 77. Topologie redondante double domaine

```
              ┌──────────┐
              │ Serveur  │
              │ HBA 16e  │
              └──┬───┬───┘
                 │   │  (domaine A / domaine B)
        ┌────────▼┐ ┌▼────────┐
        │ Expdr A │ │ Expdr B │   ◀── deux expanders par JBOD
        └────────┬┘ └┬────────┘
                 │   │
          disques dual-port (SAS)
```

Chaque disque SAS dual-port est vu par les deux domaines. Un câble ou un
expander qui meurt = bascule transparente (avec multipath, section 36).
**Obligatoire** pour tout JBOD en production sérieuse. Ne fonctionne
**qu'avec des disques SAS** (le SATA n'a qu'un port).

## 78. Zoning d'expander

Le zoning découpe un expander en groupes isolés : par exemple, 1 JBOD de
60 baies partagé entre **2 serveurs** (30 baies chacun), chacun ne voyant
que ses disques. Utile en cluster 2 nœuds actif/actif ou pour isoler un
domaine de test. Mal configuré, c'est aussi le meilleur moyen de rendre
la moitié des disques invisibles — documente le plan de zoning.

## 79. SES-3 : le monitoring du JBOD

SCSI Enclosure Services : températures, ventilateurs, alimentations, état
des baies, LEDs de localisation. Exposé via la HBA (`storcli`, `sg_ses`).
**Branche le SES à ta supervision** : un ventilateur de JBOD mort un
vendredi soir = 60 disques qui cuisent le week-end. Seuil d'alerte :
température interne > 45 °C.

## 80. Limite n°1 : la bande passante partagée (calcul)

Formule : `bande_passante_amont / nombre_de_disques_actifs = débit/disque`.
Exemple : 8 voies 12G = 9,6 Go/s ; 60 HDD en rebuild simultané à 250 Mo/s
chacun = 15 Go/s demandés → **le lien amont sature à 64 % du besoin**.
En régime normal (quelques disques actifs), pas de problème. En rebuild
généralisé ou en resilver ZFS, le temps de reconstruction **double**.
Dimensionne l'amont pour le **pire cas**, pas le cas nominal.

## 81. Limite n°2 : le nombre de disques

Au-delà de ~120-200 disques par HBA, le firmware de la HBA et les temps
d'inventaire (découverte SAS) deviennent pénibles : boot de plusieurs
minutes, `storcli show all` qui rame. Règle : **1 HBA 16 voies pour
60-120 disques max** en production.

## 82. Latence ajoutée par l'expander

Chaque traversée d'expander ajoute quelques microsecondes. En cascade
(HBA → exp1 → exp2 → disque), on peut ajouter 10-20 µs aller-retour.
Négligeable pour du HDD (ms), **mesurable** pour du NVMe/SAS-SSD à
faible latence (< 100 µs). Pour du flash performant : pas de cascade,
backplane direct.

## 83. Alimentations et refroidissement du JBOD

Un JBOD 60 baies : 2+ alimentations redondantes (souvent 1200-1600 W),
ventilateurs hot-swap. Le JBOD **ne s'arrête pas proprement** tout seul :
en cas de coupure, c'est la tête (serveur) qui doit gérer l'arrêt via
NUT/stonith. Prévois le JBOD dans le plan d'arrêt électrique (lien avec
ton guide onduleurs : même onduleur, séquence d'arrêt documentée).

## 84. Poids et manutention

Rappel section 26, aggravé : un JBOD 60 baies plein, c'est le même poids
qu'un serveur 4U (~60 kg) **sans les poignées ni l'équilibrage d'un
serveur**. Certains JBOD n'ont pas de rails coulissants : maintenance =
sortir le châssis. Vérifie ce point **avant** l'achat.

## 85. Firmware d'expander

L'expander a son firmware, avec ses bugs (perte de disques fantôme,
négociation bloquée). Symptôme typique : des disques qui « disparaissent
et réapparaissent ». Avant de suspecter les disques, **vérifie la version
firmware de l'expander** et les errata du constructeur. Flash en fenêtre
de maintenance : un flash d'expander coupe tous ses disques.

## 86. Câblage : bonnes pratiques

- Étiquette **chaque** câble (A1, A2, B1, B2 : domaine + numéro).
- Ne mélange pas les générations sur un même domaine sans test.
- Serre les vis des SFF-8644/8674 (ils se débranchent avec les vibrations).
- Photo du câblage dans la doc d'exploitation, mise à jour à chaque
  intervention.
- Un câble de rechange de chaque type **dans la baie**, pas au dépôt.

## 87. Exemple BOM : extension JBOD 60 baies

| Ligne | Qté |
|---|---|
| JBOD 60 baies 12G double expander, 2×1600 W | 1 |
| Câbles SFF-8644 2 m | 4 (2 par domaine) |
| HDD 20 To CMR | 60 |
| Rails + kit | 1 |

Coût disques seul : 60 × 20 To × ~18 €/To ≈ 21 600 € (ordre de grandeur,
**à vérifier**). Le JBOD nu (sans disques) : **à vérifier** sur devis.

## 88. Tableau : JBOD vs tout-en-un

| Besoin | JBOD + tête | Serveur 60 baies |
|---|---|---|
| €/To capacitaire | souvent meilleur | plus simple |
| Domaine de panne | tête unique | nœud unique (pareil) |
| Évolutivité | ajoute des JBOD | ajoute des nœuds |
| Ops | 2 équipements à gérer | 1 seul |

---

# PARTIE G — RAID vs ERASURE CODING vs JBOD

## 89. RAID : le principe en une phrase

Le RAID répartit données + parité sur N disques pour survivre à 1-2 pannes,
au prix d'un surcoût et d'un rebuild. Il ne protège **ni** de la corruption
silencieuse (sans checksums), **ni** de la perte du site, **ni** de l'erreur
humaine (`rm -rf`).

## 90. Tableau RAID 0 / 1 / 10

| Niveau | Surcoût | Pannes tolérées | Usage |
|---|---|---|---|
| RAID 0 | 0 % | 0 | scratch, cache (données reproductibles) |
| RAID 1 | 50 % | 1 par miroir | boot, petits volumes critiques |
| RAID 10 | 50 % | 1+ (selon placement) | bases OLTP, perf + résilience |

## 91. RAID 5 : le write hole et les URE

Le RAID 5 (1 parité) a deux défauts mortels sur gros disques :
1. **Write hole** : coupure pendant l'écriture → parité incohérente
   (atténué par journal/cache protégé, jamais totalement supprimé).
2. **URE pendant le rebuild** : avec des disques 20 To (BER ~10⁻¹⁴),
   la probabilité de rencontrer une erreur de lecture irrécupérable
   pendant un rebuild de 20 To est **significative** → rebuild qui échoue
   → volume perdu. **Règle : plus de RAID 5 au-delà de ~4-6 To par disque.**

## 92. RAID 6 : la double parité

Deux parités → survit à 2 pannes. Le standard capacitaire HDD en RAID
matériel. Surcoût : 2 disques par groupe. Reste exposé au temps de rebuild
(section 93) et ne scale pas au-delà d'une baie. Pour du multi-nœuds :
l'erasure coding logiciel (section 94) le remplace.

## 93. Temps de rebuild : le calcul qui fait peur

`temps = capacité_disque / débit_rebuild_effectif`.
Exemple : 20 To à 200 Mo/s effectifs (rebuild partagé) = 100 000 s ≈
**28 heures**. Pendant ces 28 h, le groupe est dégradé ; une 2e panne
(sur RAID 5) ou 3e (sur RAID 6) = perte. Avec 60 disques de 20 To, la
probabilité d'une 2e panne pendant la fenêtre n'est pas négligeable :
c'est l'argument mathématique pour l'erasure coding distribué.

## 94. Erasure coding : le principe

On découpe les données en **k** fragments + **m** fragments de parité
(Reed-Solomon). N'importe quels k fragments parmi k+m reconstruisent les
données. Surcoût = m/(k+m). Tolère **m** pannes **distribuées sur le cluster**,
pas sur un groupe de disques.

## 95. Profils EC courants

| Profil | Surcoût | Tolère | Usage typique |
|---|---|---|---|
| 4+2 | 33 % | 2 pannes | petit cluster, objet |
| 8+3 | 27 % | 3 pannes | standard S3/RGW |
| 8+2 | 20 % | 2 pannes | froid, gros cluster |

Comparé à réplica 3 (surcoût 200 %), l'EC divise le coût capacitaire par ~2.
Prix à payer : **CPU** (encodage), **latence d'écriture**, et rebuilds
gourmands en réseau.

## 96. Surcoût comparé (tableau)

| Méthode | Surcoût | 100 To utiles → brut |
|---|---|---|
| Réplica 3 | 200 % | 300 To |
| EC 4+2 | 50 % | 150 To |
| EC 8+3 | 37,5 % | 137,5 To |
| RAID 6 (8+2 local) | 25 % | 125 To |

Le RAID 6 local reste le moins cher en brut, mais il ne protège que dans
la baie. L'EC protège dans tout le cluster : ce n'est pas le même service.

## 97. JBOD + réplication logicielle

Le « JBOD » comme stratégie = pas de RAID du tout, la redondance est
gérée par le logiciel (Ceph réplica, ZFS copies=2, MinIO erasure sets).
Avantage : le logiciel voit les disques et leurs erreurs. Inconvénient :
il faut un logiciel qui sait le faire (pas un simple montage ext4).

## 98. Comparatif honnête

| Critère | RAID HW | EC logiciel | JBOD + réplica SW |
|---|---|---|---|
| Surcoût typique | 25-50 % | 27-50 % | 100-200 % |
| Échelle | 1 serveur | cluster | cluster |
| Rebuild | lent, local | rapide, distribué | rapide, distribué |
| Corruption silencieuse | non détectée | détectée (checksums) | détectée |
| Complexité ops | faible | élevée | moyenne |
| Coût logiciel | carte RAID | expertise | expertise |

## 99. Quand choisir le RAID hardware

- Serveur unique, applicatif classique, pas de SDS (PME, boot, petits volumes).
- RAID 1 (boot/OS) et RAID 10 (bases sur serveur unique).
- Quand l'équipe n'a pas l'expertise SDS et que le besoin tient dans une baie.
**Jamais** sous Ceph/ZFS/vSAN sur les disques de données.

## 100. Quand choisir l'erasure coding

- Objet/S3, backup, archivage : données froides, gros volumes, écriture
  peu fréquente → EC 8+3.
- Cluster ≥ 8-10 nœuds (l'EC a besoin de largeur).
- Quand le €/To prime sur la latence d'écriture.

## 101. Quand choisir JBOD + réplication

- Bloc (RBD, iSCSI) et fichiers : la réplication 3x reste le standard
  (latence d'écriture faible).
- Petits clusters (< 8 nœuds) où l'EC serait trop étroit.
- Workloads mixtes sans expertise EC.

## 102. RAID et SSD : particularités

Le rebuild SSD est rapide (pas de mécanique), mais :
- l'usure s'accélère pendant le rebuild (écritures massives),
- le RAID 5/6 sur SSD QLC à faible endurance = usure prématurée,
- le TRIM/discard ne traverse pas toujours la couche RAID → perfs qui
  se dégradent avec le temps. Vérifie le support du TRIM chez le
  constructeur de la carte.

## 103. RAID et SMR : ne pas

Les disques SMR (enregistrement magnétique shingled, ex. certains
Barracuda grand public) sont **incompatibles** avec le RAID et ZFS/Ceph
en écriture aléatoire : le réécriture de bandes fait chuter le débit à
quelques Mo/s. Pour le datacenter : **CMR uniquement** (Exos, Ultrastar
DC, IronWolf Pro). Le SMR n'a sa place qu'en écriture séquentielle pure
(archivage WORM maîtrisé).

## 104. Matrice de décision finale

```
Une seule baie, équipe réduite ──▶ RAID 1/10/6 matériel
Cluster, bloc/fichier, latence ──▶ Ceph réplica 3 / ZFS miroir
Cluster, objet/froid, €/To ──▶ Ceph EC 8+3 / MinIO erasure sets
Données reproductibles ──▶ RAID 0 / JBOD pur (avec réplication ailleurs)
```

---

# PARTIE H — CEPH FULL NVMe : DESIGN COMPLET

## 105. Architecture Ceph en 30 secondes

```
 ┌─────┐  ┌─────┐  ┌─────┐
 │ MON │  │ MGR │  │ RGW │   ← contrôle (léger)
 └──┬──┘  └──┬──┘  └──┬──┘
    └────────┼────────┘
        ┌────▼────┐
        │  RADOS  │  ← objet distribué, CRUSH
        └────┬────┘
   ┌─────────┼──────────┐
   ▼         ▼          ▼
 ┌────┐   ┌────┐    ┌────┐
 │OSD │   │OSD │    │OSD │  ← 1 daemon par disque
 └────┘   └────┘    └────┘
 RBD (bloc)   CephFS (fichier)   RGW (objet S3)
```

MON : quorum (3 ou 5). MGR : métriques, autoscaler. OSD : le daemon qui
sert un disque. **Règle d'acier : 1 OSD = 1 disque physique** (sauf cas
exotiques). Tout le dimensionnement part de là.

## 106. Pourquoi le NVMe change le design Ceph

Avec des HDD, le disque est le goulot (200 IOPS). Avec des NVMe Gen5
(2 600K IOPS, 14 Go/s), le goulot devient **le CPU, la RAM et le réseau**.
Un cluster full NVMe mal dimensionné en réseau/CPU est un cluster qui
coûte cher pour servir du 25 Gb/s par nœud. Le design full NVMe =
design **réseau + CPU** d'abord, disques ensuite.

## 107. 1 OSD par disque : la règle et ses exceptions

Un OSD par SSD NVMe. Exceptions :
- 2 OSD sur un très gros SSD (30 To+) pour mieux répartir les PG —
  technique avancée, à tester (le WAL/DB partagé peut devenir un goulot).
- Jamais 1 OSD pour 2 SSD (sauf LVM stripé, déconseillé : tu perds la
  granularité de panne).
En full NVMe : **simple, 1:1**.

## 108. PG par OSD : les chiffres officiels (vérifié)

Documentation Ceph / Red Hat (vérifié le 27/09/2026) :
- `mon_target_pg_per_osd` = **100 par défaut**,
- **200 recommandé** pour la plupart des clusters (sauf les plus petits),
- **> 500 = trop** : trafic de peering et RAM excessifs.
Avec le balancer, un cluster démarre vers 50-70 PG/OSD puis l'autoscaler
ajuste. Formule manuelle : `PG ≈ (100 × OSD) / taille_réplica`.

## 109. Calcul PG : exemple chiffré

Cluster : 6 nœuds × 16 NVMe = 96 OSD, réplica 3.
`PG = (100 × 96) / 3 = 3200` → arrondi à la puissance de 2 : **4096**
(ou 2048 si petit). Vérification : 4096 × 3 / 96 = **128 PG/OSD** —
dans la fourchette 100-200. En pratique : laisse **l'autoscaler en mode
`on`** (défaut sain) et marque les gros pools `bulk: true`.

## 110. Autoscaler : on / warn / off

| Mode | Comportement |
|---|---|
| `on` | ajuste pg_num automatiquement (défaut conseillé) |
| `warn` | propose, tu valides (clusters sensibles) |
| `off` | manuel uniquement (experts, calcul fait) |

Deux règles vérifiées (docs + retours terrain 2026) :
- pendant une **mise à jour**, l'autoscaler est en pause (un split de PG
  en pleine upgrade peut ajouter des jours sur un gros cluster),
- `target_size_ratio` > pg_num manuel : donne à l'autoscaler la part
  attendue du pool au lieu de coder des PG en dur.

## 111. RAM par OSD : règle de dimensionnement

Ordre de grandeur usuel (à vérifier selon version Ceph — Reef/Squid) :
- **HDD : ~4-5 Go par OSD** (RocksDB + cache),
- **NVMe : ~2-4 Go par OSD** (moins de latence à masquer, mais plus de PG).
Exemple : 16 NVMe × 3 Go = 48 Go + OS/MON/MGR (~16 Go) + marge → **128 Go
minimum, 256 Go confortable** par nœud 16 baies. Le manque de RAM se voit
en premier sur les pics de recovery (OOM killer sur les OSD).

## 112. CPU : cœurs par OSD

Ordre de grandeur (à vérifier sur ta version) :
- HDD : ~0,5-1 cœur par OSD,
- **NVMe : ~1-2 cœurs par OSD** (chiffrement msgr, BlueStore, recovery).
Exemple : 16 NVMe → 16-32 cœurs. Un double socket 16c/32c (32c/64t) est
le standard raisonnable pour un nœud 16-24 NVMe. En dessous, le recovery
sature le CPU avant le réseau.

## 113. Réseau : débit par OSD (le calcul clé)

En réplica 3, chaque octet écrit par le client génère ~3 octets de trafic
cluster (réplication) + overhead. Formule terrain :
`débit_réseau_nœud ≈ débit_disques_nœud × facteur_réplication × 1,2`.
Exemple : 16 NVMe × 3 Go/s (soutenable, pas le pic) = 48 Go/s de disques ;
× 3 × 1,2 = **172 Go/s = 1 376 Gb/s** théoriques. Personne ne met 14×100G
par nœud : en pratique on dimensionne sur le **débit client cible**
(voir section 118), pas sur le pic disque.

## 114. Réseau : topologie recommandée

| Lien | Débit 2026 | Usage |
|---|---|---|
| Client (public) | 2× 25G ou 2× 100G | accès RBD/RGW/CephFS |
| Cluster (privé) | 2× 100G | réplication, recovery, heartbeat |
| (Option) | 400G | gros nœuds 32 NVMe Gen5 |

Sépare **toujours** le réseau cluster du réseau client (VLAN ou physique).
Le recovery d'un nœud 32×7,68 To peut saturer 100G pendant des heures :
si c'est le même lien que les clients, tout le monde le sent.

## 115. Dimensionnement « 100 To utiles » : hypothèses

- Besoin : **100 To utiles** (Tio, après conversion !), workload mixte VM.
- Protection : réplica 3 (latence faible).
- Disques : NVMe E3.S 7,68 To Gen5 (6,99 Tio bruts).
- Overhead : 10 % (métadonnées, marge de remplissage — on ne remplit
  jamais un cluster Ceph au-delà de ~70-80 %).

## 116. Dimensionnement : nombre de disques

Brut nécessaire = 100 × 3 × 1,1 ≈ **330 To** → / 6,99 Tio ≈ **48 disques**.
Arrondi pratique : **48 NVMe de 7,68 To**. Avec des 15,36 To : 24 disques
(moins de granularité PG — voir section 108 : 24 OSD, c'est peu pour
4096 PG ; préfère plus de petits disques que peu de gros).

## 117. Dimensionnement : nombre de nœuds

Contraintes : failure domain = le nœud ; réplica 3 → **minimum 3 nœuds**,
recommandé 4-5 pour survivre à 1 panne + 1 maintenance.
Choix : **4 nœuds × 12 NVMe** = 48 OSD.
Par nœud : 12 × 6,99 = 83,9 Tio bruts ; cluster brut = 335,5 Tio ;
utiles ≈ 335,5 / 3 × 0,9 ≈ **100 Tio**. Le calcul tombe juste.
Alternative 5 nœuds × 10 : plus résilient, un peu plus cher.

## 118. Dimensionnement : réseau chiffré

Débit client cible : 4 nœuds × 25 Go/s (2×100G LACP par nœud, réaliste
soutenable) = **100 Go/s agrégés** côté client. Réseau cluster : 2×100G
par nœud également (le recovery doit suivre). Total par nœud : **4×100G**
(2 client + 2 cluster) ou 2×100G partagés avec QoS stricte (déconseillé
en production). Budget switch : 2× 32 ports 100G (MLAG) — **à vérifier**
sur devis (~15-30 k€ pièce en ordre de grandeur public).

## 119. Dimensionnement : RAM et CPU chiffrés

Par nœud (12 NVMe) : RAM = 12 × 3 Go + 16 Go système ≈ **64 Go → 128 Go
installés** (marge recovery). CPU : 12 × 1,5 cœur ≈ 18 cœurs → **2× 16c**
ou 1× 32c moderne. Conso nœud estimée : voir section 187.

## 120. Exemple BOM : nœud Ceph NVMe (×4)

| Ligne | Qté/nœud | Ordre de grandeur |
|---|---|---|
| Châssis 2U 12-24 baies E3.S/U.2 Gen5 | 1 | à vérifier |
| 2× CPU 16c / 1× 32c | 1 lot | à vérifier |
| 128 Go DDR5 ECC | 1 lot | à vérifier |
| 12× NVMe 7,68 To Gen5 E3.S | 12 | ~350-600 €/To → 32 000-55 000 € |
| 2× NIC 100G (client) + 2× NIC 100G (cluster) | 4 | à vérifier |
| 2× alimentations Titanium | inclus | — |

Ordre de grandeur par nœud (sans les NIC/switch) : **40-70 k€** ;
cluster 4 nœuds : **160-280 k€** hors réseau. **À vérifier** sur devis
(allocation tendue sept. 2026).

## 121. Coût au To utile : l'indicateur à suivre

`€/To utile = coût total cluster / To utiles`. Exemple ci-dessus :
~200 k€ / 100 To ≈ **2 000 €/To utile** (ordre de grandeur, à vérifier).
Compare toujours en €/To **utile** (pas brut) et en €/To/an sur 5 ans
en ajoutant énergie + maintenance. Le NVMe full perf coûte ~5-10× le
HDD capacitaire au To : c'est normal, ce n'est pas le même service.

## 122. Recovery et backfill : l'épreuve du feu

Quand un OSD meurt, Ceph recopie ses PG sur les survivants (backfill).
Sur full NVMe, le recovery est **très rapide** (bon) mais **sature le
réseau et le CPU** (mauvais si sous-dimensionnés). Réglages : limite
`osd_max_backfills` et `osd_recovery_max_active` pendant les heures
ouvrées, ouvre les vannes la nuit. Teste **avant** la production :
tue un OSD en burn-in et mesure l'impact client.

## 123. BlueStore : ce qu'il faut savoir

BlueStore écrit directement sur le block device (pas de filesystem
intermédiaire), avec RocksDB pour les métadonnées (omap) et checksums
intégraux. Filestore est **obsolète** (vérifié : docs 2026). Conséquence
pratique : pas de `mkfs` sur les disques OSD, et les métadonnées
RocksDB méritent un device rapide (d'où le block.db en hybride).

## 124. WAL/DB en full NVMe : colocalisés

En full NVMe, pas de device séparé : `block.wal` et `block.db` sont
**colocalisés** sur le même SSD (comportement par défaut de BlueStore
quand il n'y a pas de mix de médias — vérifié). Ne crée pas de LV
séparés « pour optimiser » : tu fragmenterais pour rien.

## 125. mClock et QoS (aperçu)

dmClock/mClock : l'ordonnanceur QoS de Ceph (client vs recovery vs
scrub). Sur full NVMe, active et profile le mClock : c'est ce qui
empêche un scrub ou un backfill d'écraser la latence client. Profils :
`high_recovery_ops` en journée, équilibrage la nuit. À tester par
workload — il n'y a pas de profil universel.

## 126. RGW sur NVMe : particularités

Le RGW (S3) génère énormément de petites clés omap → RocksDB sollicité.
En full NVMe ça tient, mais prévois : **plus de RAM** (cache omap),
et si tu fais de l'EC 8+3 sur RGW, le CPU d'encodage par nœud (voir
section 112, prends la fourchette haute).

## 127. CephFS sur NVMe

CephFS = MDS (métadonnées) + RADOS (données). Sur NVMe : MDS très
réactifs, idéal pour homes, builds, VDI. Points d'attention : dimensionne
les **MDS** (RAM ! les MDS adorent la RAM — 64 Go+ par MDS actif),
et sépare le pool `cephfs_metadata` (réplica 3, petits objets) du pool
data (EC possible pour le froid).

## 128. RBD sur NVMe

Le cas roi du full NVMe : images VM (Proxmox !), bases. Latence typique
bien réglée : < 1 ms p99 en lecture. Réglages qui comptent :
`rbd_cache` côté client, taille d'objet 4 Mo (défaut sain), et surtout
**pas de cache tiering** (déprécié — section 137).

## 129. Erasure coding sur NVMe : le piège latence

L'EC sur NVMe économise des To mais **ajoute de la latence d'écriture**
(encodage + fan-out réseau k+m). Sur du bloc synchrone (VM, bases),
l'EC 4+2 peut doubler la latence d'écriture vs réplica 3. Règle : **EC
pour l'objet et le froid, réplica pour le bloc chaud**. Mesure avant
de décider : `rados bench` sur les deux profils.

## 130. Monitoring : les métriques qui comptent

| Métrique | Seuil d'alerte |
|---|---|
| `ceph health` != HEALTH_OK | immédiat |
| PG `degraded` / `stuck` | immédiat |
| Latence op OSD p99 (commit/apply) | > 50 ms NVMe = anomalie |
| `bluestore_slow_ops` | > 0 répété = disque qui rame |
| Remplissage pool | > 70 % warning, > 85 % critique |
| Recovery/backfill en cours | info, corréler avec la latence client |

Resserre `bluestore_slow_ops_warn_lifetime` (défaut 86400 s) pour détecter
un SSD qui rame **avant** la cascade (retour terrain vérifié 2026 :
lifetime 300 s, threshold 5 — à ajuster).

## 131. Mises à jour : l'autoscaler en pause (vérifié)

Pendant une upgrade Ceph, l'autoscaler est **mis en pause automatiquement**
par cephadm (sauf `mgr/cephadm/pg_autoscale_during_upgrade`). Un split de
PG à mi-upgrade peut ajouter des jours sur un gros cluster (retour
terrain 2026). Procédure : upgrade → vérifie HEALTH_OK → réactive →
laisse l'autoscaler converger → seulement ensuite la charge.

## 132. Checklist design full NVMe

- [ ] 1 OSD = 1 SSD, PG/OSD dans 100-200
- [ ] Autoscaler `on`, pools `bulk: true`
- [ ] RAM : 2-4 Go/OSD + 16 Go système
- [ ] CPU : 1-2 cœurs/OSD
- [ ] Réseau cluster séparé, 100G mini par nœud
- [ ] Recovery testé en burn-in (kill OSD)
- [ ] mClock profilé, monitoring en place
- [ ] Remplissage max 70-80 % en exploitation

---

# PARTIE I — CEPH HYBRIDE (SSD + HDD)

## 133. Principe de l'hybride

Les HDD portent les données (block), les SSD portent les métadonnées
RocksDB + le journal (block.db, qui absorbe le block.wal). Résultat :
la latence des petites écritures devient celle du SSD, le €/To reste
celui du HDD. C'est **l'architecture Ceph capacitaire standard**.

## 134. Sizing block.db / block.wal (vérifié)

Documentation Ceph actuelle (vérifié le 27/09/2026 via docs + retours) :
- **≥ 2,5 %** de la taille du `block` (RocksDB compresse depuis Squid —
  l'ancienne règle « 4 % partout » est obsolète),
- **≥ 4 % pour RGW** (volume de clés omap),
- **1-2 % suffisent pour RBD**.
Exemple : HDD 20 To en RBD → db de 400 Go ; en RGW → 800 Go. **Ne
sous-dimensionne jamais** : un block.db plein = OSD en erreur.

## 135. Ratio OSD par device rapide (vérifié)

- **4-5 OSD HDD par SSD SATA**,
- **≤ 15 OSD HDD par NVMe**.
Exemple : 60 HDD → 12-15 SSD SATA de db, ou 4 NVMe. Le device rapide est
un **domaine de panne** : s'il meurt, tous ses OSD meurent → intègre-le
dans le design CRUSH (ne mets pas les 4 NVMe de db sur les mêmes 4 nœuds
que... enfin, réfléchis à la blast radius).

## 136. Le device rapide comme failure domain

Si 1 NVMe porte le db de 15 OSD et qu'il meurt : 15 OSD down d'un coup =
potentiellement des PG indisponibles si la réplication est mal répartie.
Mitigations : device classes CRUSH séparées, `ceph osd out` rapide
automatisé, et stock de SSD de rechange **identiques** (même taille de
db → remplaçable sans recalcul).

## 137. Cache tiering : ne pas utiliser

Le cache tiering (pool SSD chaud devant pool HDD froid) est **déprécié/
abandonné** dans Ceph moderne : bugs de promotion, pas de maintenance.
Les alternatives saines : **device classes + règles CRUSH** (données
chaudes sur SSD, froides sur HDD, déplacement par politique applicative),
ou deux clusters. Si un commercial te propose du cache tiering Ceph en
2026 : non.

## 138. CRUSH : les device classes

```
ceph osd crush set-device-class hdd osd.0
ceph osd crush set-device-class ssd osd.60
```
Puis des règles `replicated_rule_ssd` / `replicated_rule_hdd`. C'est le
mécanisme propre pour : pool RBD chaud sur SSD, pool backup sur HDD,
le tout dans **un seul cluster**.

## 139. Exemple de règle CRUSH hybride

```
rule rbd-ssd {
    ruleset 1
    type replicated
    min_size 1 max_size 10
    step take default class ssd
    step chooseleaf firstn 0 type host
    step emit
}
```
Traduction : les PG de ce pool ne sont placés que sur des OSD de classe
`ssd`, un par host. Duplique pour `hdd`. Vérifie avec
`ceph osd crush rule dump`.

## 140. Dimensionnement hybride chiffré : 500 To utiles froids

Hypothèses : 500 To utiles, EC 8+3 (37,5 % de surcoût), HDD 20 To.
Brut = 500 × 1,375 × 1,1 ≈ 756 To → **40 HDD de 20 To** (800 To bruts).
Nœuds : 5 nœuds × 8 HDD (blast radius raisonnable, EC 8+3 tient sur 5
nœuds avec min_size adapté — vérifie k+m ≤ nœuds).
DB : 40 × 20 To × 2,5 % = 20 To de db → **3 NVMe de 7,68 To** (ou 2×
15,36 To), répartis. RAM : 40 × 4 Go + système → **256 Go/nœud**.
Réseau : 2×25G par nœud suffisent (froid), cluster séparé.

## 141. Piège : le block.db plein

Symptômes : OSD qui flap, erreurs ENOSPC alors que le HDD est à 50 %.
Cause : db sous-dimensionné ou workload plus « omap » que prévu (RGW !).
Prévention : dimensionne à **4 % pour RGW dès le départ**, monitor
`ceph daemon osd.X perf dump` (espace db), et prévois la procédure
d'extension (recréation d'OSD — il n'y a pas d'extension en place simple).

## 142. Piège : le WAL sur device lent

Ne mets **jamais** le block.wal sur le HDD « pour économiser un SSD » :
le WAL est sur le chemin critique de chaque écriture. Le couple
wal+db va **toujours** sur le rapide (d'ailleurs le wal est absorbé
dans le db par défaut en BlueStore moderne).

## 143. Piège : mélanger les tailles de HDD

Des OSD de 12 To et de 20 To dans le même pool : Ceph remplit au
pourcentage, les 12 To se remplissent (en %) aussi vite → le cluster
s'arrête sur les petits disques. Règle : **une taille de disque par
pool**, ou des pools séparés par taille (device class custom ou
pools dédiés).

## 144. Migration hybride → full NVMe

Stratégie : ajoute des nœuds NVMe (device class `nvme`), crée les
nouveaux pools dessus, migre les données (rbd migration, RGW
resharding, rsync pour CephFS), puis retire les nœuds HDD avec
`ceph osd out` + rebalance. **Jamais de bascule « big bang »** :
la migration prend des jours sur des centaines de To — c'est normal,
planifie-la.

## 145. Cas d'usage : backup sur hybride

Backup (Veeam, Proxmox Backup Server, Borg) : écritures séquentielles,
lectures rares → **hybride HDD + db SSD**, EC 8+3 ou réplica 2 selon
criticité. Le db SSD absorbe les pics d'écriture des fenêtres de backup.
Dimensionne le réseau pour la **fenêtre de backup** (ex. 8 h pour
50 To = ~1,8 Go/s soutenus).

## 146. Cas d'usage : CephFS froid

Données utilisateurs, archives : pool data en EC sur HDD, pool metadata
en réplica 3 sur SSD (les métadonnées sont petites et chaudes). Le MDS
apprécie énormément un pool metadata sur SSD : le `ls -R` d'un gros
volume passe de minutes à secondes.

## 147. Ordre de grandeur prix hybride

Exemple section 140 : 40× HDD 20 To (~18 €/To ≈ 14 400 €) + 3× NVMe
7,68 To (~10 000 €) + 5 nœuds modestes + réseau 25G. Total ordre de
grandeur : **60-100 k€** pour 500 To utiles → **~120-200 €/To utile**,
soit ~10× moins cher que le full NVMe. **À vérifier** sur devis.

## 148. Checklist hybride

- [ ] block.db ≥ 2,5 % (4 % RGW), jamais sur HDD
- [ ] ≤ 15 OSD HDD par NVMe de db
- [ ] Device classes + règles CRUSH par tier
- [ ] Pas de cache tiering
- [ ] Une taille de HDD par pool
- [ ] Monitoring espace db

---

# PARTIE J — ALTERNATIVES À CEPH (SDS DU MARCHÉ)

## 149. Panorama : tableau comparatif (existence vérifiée le 27/09/2026)

| Solution | Type | Licence | Contrats | Positionnement |
|---|---|---|---|---|
| Ceph | SDS unifié | LGPLv2.1 | bloc/fichier/objet | généraliste, expert |
| MinIO | objet S3 | AGPLv3 | objet | S3 simple et rapide |
| Garage | objet S3 | AGPLv3 | objet | léger, géo-distribué |
| SeaweedFS | objet/fichier | Apache 2.0 | objet/fichier | petits fichiers, simple |
| RustFS | objet S3 | Apache 2.0 | objet | successeur potentiel MinIO, jeune |
| Apache Ozone | objet/fichier | Apache 2.0 | objet/fichier | Hadoop, exa-échelle |
| OpenStack Swift | objet | Apache 2.0 | objet | clouds privés historiques |
| JuiceFS | fichier sur objet | Apache 2.0 | fichier | POSIX au-dessus de S3 |
| Lustre | fichier parallèle | GPLv2 | fichier | HPC |
| WEKA | fichier parallèle | propriétaire | fichier/objet | IA/GPU, ultra-faible latence |
| VAST Data | fichier/objet | propriétaire | fichier/objet | IA, all-flash |
| Scality | objet | propriétaire | objet | S3 entreprise, multi-région |
| Cloudian | objet | propriétaire | objet | S3, backup, anti-ransomware |
| IBM Spectrum Scale | fichier | propriétaire | fichier | HPC/IA, POSIX |

Toutes ces solutions **existent** (vérifié via comparatifs et docs, 27/09/2026).
GlusterFS existe aussi mais est en déclin (Red Hat l'a déprécié au profit de Ceph/ODF).

## 150. MinIO : l'état en 2026

MinIO = le S3 self-hosted le plus simple : erasure coding inline, pas
d'index séparé, déploiement en minutes. Points de vigilance 2026
(presse/communauté, vérifié) : licence **AGPLv3** (contamination IP à
évaluer avec ton juridique), et un essoufflement perçu de la version
communautaire (des équipes migrent vers Garage/SeaweedFS/RustFS ou Ceph
RGW). Pour un S3 simple, mono-site, équipe réduite : reste un choix
valable. Pour du multi-site critique : évalue Ceph RGW ou Scality.

## 151. Garage : le léger géo-distribué

Écrit en Rust, **très faible empreinte** (tourne sur ARM, petits nœuds),
réplication multi-sites pensée dès le départ. Limites : pas pour le
multi-pétaoctet, S3 partiel. Idéal : edge, sites distants, S3 léger
d'entreprise. Licence AGPLv3.

## 152. SeaweedFS : les petits fichiers

Modèle inspiré de Haystack (Facebook) : excellent sur les **milliards
de petits fichiers** où Ceph RGW peine (overhead par objet). Licence
Apache 2.0 (business-friendly), S3 + FUSE + HDFS. À évaluer quand ton
workload = beaucoup de petits objets.

## 153. RustFS : le jeune challenger

Rust, Apache 2.0, annoncé ~2,3× plus rapide que MinIO sur petits payloads
(selon le projet — **à vérifier** en bench propre), zéro télémétrie.
Statut : jeune (alpha 2026), une faille de sécurité a refroidi des
adoptants. À surveiller, pas à mettre en production critique en 2026.

## 154. Apache Ozone : l'exa-échelle Hadoop

Ozone = l'objet « Hadoop moderne » : S3 + HDFS API, **forte cohérence**,
échelle exaoctet / dizaines de milliards de clés (vérifié : comparatif
Apache Ozone). Si ton écosystème est Hadoop/Spark : Ozone plutôt que
Ceph RGW. Sinon : complexité injustifiée.

## 155. OpenStack Swift : l'historique

L'objet d'OpenStack : anneaux (rings), cohérence à terme, multi-tenant.
Toujours maintenu, pertinent si tu es déjà sur OpenStack. Sinon, les
alternatives modernes (RGW, SeaweedFS) sont plus simples à opérer.

## 156. JuiceFS : du POSIX sur de l'objet

JuiceFS met un filesystem POSIX **au-dessus** d'un stockage objet (S3,
etc.) avec Redis/métadonnées séparées. Cas d'usage : partager un volume
POSIX entre clouds, ou donner du filesystem à des applis sur un S3
existant. Ce n'est pas un SDS complet : il faut l'objet en dessous.

## 157. Lustre : le HPC

Le filesystem parallèle des supercalculateurs : débit agrégé monstrueux,
POSIX, mais administration experte et pas d'objet. Pour un cluster IA/GPU
où le checkpoint/restore et le training dictent tout : Lustre ou WEKA,
pas Ceph.

## 158. WEKA : la performance propriétaire (vérifié)

Filesystem parallèle propriétaire, positionné **IA/HPC** : latence
ultra-faible, IOPS extrêmes, protocoles POSIX/NFS/SMB/S3. Modèle :
logiciel sur ton hardware (ou appliance). Coût : licence au To —
**à vérifier** sur devis, c'est le haut du panier tarifaire. À évaluer
quand le stockage est le goulot du training IA.

## 159. VAST Data : l'all-flash IA (vérifié)

Plateforme propriétaire **all-flash NVMe** (NFS/S3), pensée pour l'IA et
l'analytique : faible latence, réduction de données agressive. Positionné
contre les baies traditionnelles. Comme WEKA : excellent, cher, à
chiffrer contre un Ceph full NVMe bien conçu (l'écart se réduit si tu
as l'expertise Ceph en interne).

## 160. Scality : l'objet entreprise (vérifié)

S3 entreprise : multi-région, durabilité, cas backup/archive et cloud
hybride. Le choix « S3 sans surprise » quand on veut du support
contractuel et pas d'équipe SDS interne. Concurrent direct de Ceph RGW
+ support Red Hat/IBM.

## 161. Cloudian : l'objet anti-ransomware (vérifié)

S3 compatible avec fonctions **protection ransomware** (verrouillage
d'objets, immuabilité), backup et hybride. Pertinent quand la menace
n°1 est le chiffrement malveillant des backups : l'immutabilité S3
(Object Lock) est une exigence, quel que soit le produit.

## 162. IBM Spectrum Scale (ex-GPFS) : le POSIX scale-out (vérifié)

Filesystem parallèle IBM : POSIX strict, tiering par politiques, HPC/IA.
L'option « grande entreprise IBM » quand le support et la conformité
priment sur le coût. Toujours vivant en 2026 dans le HPC et la finance.

## 163. Dell PowerScale (ex-Isilon) : le NAS scale-out (vérifié)

NAS scale-out propriétaire : POSIX, milliards de fichiers, multi-Po.
Le choix « appliance qui marche » pour du fichier d'entreprise mixte
(bureautique, home, analytics) sans équipe SDS. Prix appliance :
**à vérifier**, c'est du haut de gamme.

## 164. Matrice : quand choisir quoi

| Besoin | Choix par défaut | Alternative |
|---|---|---|
| S3 simple, équipe réduite | MinIO / SeaweedFS | Ceph RGW |
| S3 critique multi-sites | Ceph RGW | Scality |
| Bloc + fichier + objet unifiés | Ceph | — |
| POSIX partagé simple | CephFS / JuiceFS | PowerScale |
| Training IA, débit extrême | WEKA / Lustre | VAST |
| Edge / ARM / léger | Garage | — |
| Backup immuable | tout S3 avec Object Lock | Cloudian |

---

# PARTIE K — FIABILITÉ : SMART, ENDURANCE, FIRMWARE, BURN-IN

## 165. La courbe en baignoire

Mortalité des disques : élevée au début (défauts de fabrication) →
plateau (pannes aléatoires) → remontée (usure). D'où le **burn-in**
(section 174) : on fait tomber les morts-nés **avant** la production,
pas pendant.

## 166. SMART : les attributs HDD qui comptent

| Attribut | Nom | Seuil d'action |
|---|---|---|
| 5 | Reallocated Sector Count | > 0 = surveiller, croissance = remplacer |
| 187 | Reported Uncorrectable Errors | > 0 = remplacer |
| 188 | Command Timeout | croissance = câble/contrôleur ou disque |
| 197 | Current Pending Sector | > 0 = surveiller de près |
| 198 | Offline Uncorrectable | > 0 = remplacer |
| 194 | Temperature | > 45 °C = refroidissement |

Automatise : `smartd` + envoi vers Zabbix. Un disque ne meurt presque
jamais sans prévenir — **si on regarde**.

## 167. NVMe Health Log : les champs critiques

`nvme smart-log` : `percentage_used` (usure), `media_errors`,
`num_err_log_entries`, `temperature`, `power_on_hours`,
`unsafe_shutdowns`. Seuil : `percentage_used` > 70 % = planifier le
remplacement ; `media_errors` > 0 répétés = RMA. Les SSD d'entreprise
exposent aussi des logs étendus (OCP 2.0 — vérifié : supporté par le
D5-P5430).

## 168. DWPD : la définition

Drive Writes Per Day : nombre de fois la capacité du disque qu'on peut
écrire par jour pendant la garantie (5 ans typique).
`TBW = DWPD × capacité × 365 × 5`.
Exemple : 7,68 To à 1 DWPD = 7,68 To/jour × 1825 j ≈ **14 Po** de TBW.

## 169. Endurance vérifiée : exemples 2026

| SSD (vérifié 27/09/2026) | Endurance | Classe |
|---|---|---|
| KIOXIA CD9P-R 7,68 To | 1 DWPD | lecture intensive |
| KIOXIA CD8P | 3 DWPD | mixte |
| Solidigm D5-P5430 30,72 To | 31,92 Po (0,58 DWPD) | QLC capacitaire |
| Solidigm D7-PS1010 1,92 To | 3 504 To (1 DWPD) | performance |

Règle d'achat : **1 DWPD** = standard mixte sain ; 3 DWPD = écriture
intensive (journaux, caches) ; < 1 DWPD (QLC) = lecture/froid uniquement.

## 170. Tableau endurance par usage

| Usage | DWPD mini conseillé |
|---|---|
| Boot / OS | 1 |
| VM mixtes | 1 |
| Bases OLTP / journaux | 3 |
| Caches / burst | 3+ (ou Optane-like / SLC) |
| Backup / objet froid | 0,5-1 (QLC OK) |
| block.db Ceph | 3 (écritures intenses) |

Le block.db d'un cluster hybride : **ne jamais mettre du QLC**
dessus — c'est le composant le plus écrit du cluster.

## 171. Over-provisioning (vérifié)

L'over-provisioning = la NAND cachée au-delà de la capacité annoncée.
Vérifié : le D7-PS1010 1,92 To expose **259,9 Go / 14,5 %** d'OP
(TechPowerUp, 27/09/2026). Plus d'OP = meilleure endurance et des
perfs soutenues plus stables. Certains SSD permettent d'augmenter
l'OP en réduisant la capacité exposée (`nvme format` avec namespace
plus petit) — utile pour transformer un SSD lecture en SSD mixte.

## 172. SLC cache : principe et limite

Beaucoup de SSD (y compris entreprise QLC) absorbent les écritures
dans un cache pseudo-SLC avant de les replier en QLC. Tant que le
cache absorbe : perfs excellentes. Quand il est plein (écriture
soutenue) : chute brutale au débit QLC natif. Pour du workload
soutenu : regarde les perfs **après** saturation du cache (les bons
tests le mesurent — vérifié : méthodologie TweakTown avec
préconditionnement).

## 173. Firmware : les pièges

- Un SSD neuf n'a pas toujours le dernier firmware **stable**
  (le « dernier » n'est pas toujours le « stable »).
- Certains bugs firmware tuent par lots (même modèle, même heure) :
  **ne jamais flasher tout le parc le même jour** ; commence par un
  nœud pilote.
- Note les versions par numéro de série dans l'inventaire.
- Les SSD grand public dans un rôle entreprise : firmwares sans
  power-loss protection → corruption en cas de coupure (voir 174).

## 174. Burn-in : la procédure

Tout disque neuf subit avant production :
1. `smartctl -t long` (HDD) ou écriture complète + lecture (SSD),
2. 2-4 cycles d'écriture/lecture complète (`badblocks -w` ou `fio`),
3. surveillance température et erreurs pendant 48-72 h,
4. `percentage_used` / SMART de référence archivé.
Un disque qui survit au burn-in sans erreur a passé le début de la
courbe en baignoire. **Ne jamais sauter le burn-in** pour « gagner
une semaine » — c'est la semaine la plus rentable du projet.

## 175. URE et BER : les ordres de grandeur

| Classe de disque | BER typique | Sens |
|---|---|---|
| HDD entreprise | 10⁻¹⁵ | 1 erreur / 125 To lus |
| HDD grand public | 10⁻¹⁴ | 1 erreur / 12,5 To lus |
| SSD entreprise | 10⁻¹⁶ - 10⁻¹⁷ | bien meilleur |

C'est ce BER qui condamne le RAID 5 sur gros disques (section 91) :
lire 20 To avec un BER de 10⁻¹⁴ pendant un rebuild, c'est jouer à la
roulette. En Ceph/ZFS, les checksums transforment l'URE en « relecture
sur une autre réplique » : incident, pas catastrophe.

## 176. MTBF et AFR : lire entre les lignes

MTBF Exos : **2 500 000 h** (vérifié). Traduction honnête : AFR
(annualized failure rate) ≈ 0,35 %/an en conditions nominales.
Sur 60 disques : 60 × 0,35 % ≈ **0,2 panne/an** — soit une panne tous
les ~5 ans par nœud. Sur 600 disques : ~2 pannes/an : il faut un
process de RMA rodé, pas de l'improvisation. La MTBF ne prédit pas
*ton* disque, elle dimensionne *ton stock de rechange*.

## 177. AFR terrain : les retours

Les études publiées (Backblaze, Google — ordres de grandeur publics) :
AFR HDD datacenter **0,5-1,5 %/an** selon modèle et température, avec
des lots à problème qui montent à 3-5 %. Règle : suis l'AFR **par
modèle et par lot d'achat** dans ton parc, et bannis les modèles qui
dévient. Un tableau de bord « pannes par modèle » vaut tous les
benchmarks.

## 178. Scrub et patrol read

Le scrub (ZFS/Ceph) lit périodiquement toutes les données et vérifie
les checksums : c'est ce qui transforme la corruption silencieuse en
incident réparable. Patrol read (RAID matériel) : l'équivalent côté
carte. **Planifie les scrubs** (hebdo sur HDD, mensuel sur SSD) et
surveille leur durée : un scrub qui s'allonge = disques qui rament.

## 179. SMART long test vs short

- Short : ~2 min, électronique + petite zone.
- Long : lecture complète de la surface (heures sur 20 To).
En burn-in : long. En production : short hebdo + long trimestriel,
**jamais pendant les fenêtres de backup**. Un long test qui trouve
des secteurs pending = remplace avant la panne.

## 180. Remplacement proactif : la politique

Seuils de remplacement **écrits** dans la doc d'exploitation :
- SMART 5/187/197/198 > 0 et croissant,
- `percentage_used` SSD > 70 %,
- température > 50 °C de façon durable,
- 2 incidents « disque lent » (slow ops) en 30 jours.
Un disque remplacé à 70 % d'usure coûte un RMA planifié ; à 100 % en
pleine nuit, il coûte une astreinte + un rebuild.

## 181. Garantie 5 ans : le standard entreprise (vérifié)

Les SSD datacenter cités (D7-PS1010, CD9P-R, D5-P5430) sont garantis
**5 ans** (vérifié 27/09/2026), comme les HDD Exos (5 ans, vérifié).
En dessous (Barracuda 24 To : 1-2 ans selon canal — vérifié), c'est du
grand public : pas de datacenter. Aligne la **durée d'amortissement**
du projet sur la garantie : 5 ans.

## 182. Checklist fiabilité

- [ ] Burn-in 48-72 h avant production, résultats archivés
- [ ] smartd/nvme-cli → supervision, seuils écrits
- [ ] Scrub planifié et surveillé
- [ ] Stock de rechange : 2-5 % du parc, modèles identiques
- [ ] Politique de remplacement proactif écrite
- [ ] Firmware : versions inventoriées, flash par vagues

---

# PARTIE L — ÉNERGIE : CONSO, REFROIDISSEMENT, ALIMENTATION

## 183. Conso par disque : le tableau (vérifié le 27/09/2026)

| Disque | Actif | Idle | Source |
|---|---|---|---|
| HDD 3,5" 7200 tr/min (20-30 To) | 8-10 W | 5-7 W | ordre de grandeur (à vérifier par modèle) |
| SSD NVMe Gen5 7,68 To (D7-PS1010) | 17-18 W | 5 W | TechPowerUp (vérifié) |
| SSD NVMe E3.S (D7-PS1010, états) | 5-25 W (5 états) | 5 W | TweakTown/Solidigm (vérifié) |
| SSD QLC 30,72 To (D5-P5430) | jusqu'à 25 W | 5 W | spec Solidigm (vérifié) |
| KIOXIA CD9P (Gen5) | ~20-23 W typ. | — | product brief (vérifié, ordre de grandeur) |
| HBA 9500-16e | 8,74 W | — | fiche produit (vérifié) |
| HBA 9500-8e | 6,12 W | — | fiche produit (vérifié) |

Message clé : **un NVMe Gen5 consomme ~2× un HDD** à l'unité, mais
stocke autant pour 3-5× moins de baies. Le bilan se fait **au To
et au nœud**, jamais au disque.

## 184. Bilan nœud 60 baies HDD (chiffré)

| Poste | Calcul | Puissance |
|---|---|---|
| 60 HDD × 9 W | 540 W | 540 W |
| 2 CPU + carte mère + RAM | — | ~300 W |
| HBA + NIC + ventilateurs | — | ~100 W |
| **Total nœud** | | **~940 W** |
| + alim Titanium (96 %) | / 0,96 | ~980 W au mur |

Soit **~16 W par baie** et ~0,8 W/To (20 To/disque). Pour 10 nœuds :
~10 kW IT, ~14-15 kW avec PUE 1,5.

## 185. Bilan nœud 12 NVMe Gen5 (chiffré)

| Poste | Calcul | Puissance |
|---|---|---|
| 12 NVMe × 18 W | 216 W | 216 W |
| 2 CPU + 128 Go DDR5 | — | ~400 W |
| 4× NIC 100G (~20 W chacun) | 80 W | 80 W |
| Ventilateurs + divers | — | ~100 W |
| **Total nœud** | | **~800 W** |

Par To utile (25 To/nœud utiles en réplica 3) : **~32 W/To utile** —
~40× plus que le HDD capacitaire. Normal : ce n'est pas le même service
(latence < 1 ms vs ~10 ms).

## 186. Spin-down : principe et pièges

Le spin-down (arrêt de rotation des HDD inactifs) économise ~5-7 W par
disque. Pièges :
- **Ceph/ZFS réveillent les disques en permanence** (scrub, heartbeat,
  métadonnées) : le spin-down ne sert quasiment que sur de l'archivage
  pur (backup immuable, WORM).
- Les cycles spin-up/down **usent** la mécanique : un disque qui
  cycle 10×/jour meurt plus vite qu'un disque qui tourne.
- Le spin-up simultané de 60 disques = **pic de courant** (~2× la conso
  nominale pendant 10-20 s) : à intégrer au dimensionnement onduleur.
Verdict : utile en archivage froid, contre-productif en Ceph actif.

## 187. Refroidissement : le calcul

Règle (section 25) : ~1,7 m³/h par watt pour ΔT = 2 °C. Nœud 60 baies
à 980 W → **~1 660 m³/h**. Vérifie aussi la **température d'entrée** :
les specs disques sont données pour 0-60 °C ambiant boîtier, mais l'AFR
grimpe au-delà de 40 °C internes. Allée froide à 24-27 °C (ASHRAE A1) :
le bon compromis énergie/fiabilité.

## 188. PUE : le rappel pour le stockage

Le stockage dense a un **bon PUE apparent** (beaucoup d'IT par m²) mais
un mauvais **W/To** en NVMe. Quand tu présentes un projet : donne les
deux. Et n'oublie pas que la clim d'un nœud 60 baies à 1 kW, c'est
~0,5 kW de froid avec un PUE de 1,5 : le nœud coûte **1,5 kW** au
compteur, pas 1 kW.

## 189. Dimensionnement électrique : exemple 4 nœuds NVMe

4 nœuds × 800 W = 3,2 kW IT. Avec PUE 1,5 : 4,8 kW. Ajoute : 2 switchs
100G (~300 W chacun), marge 30 %, pic spin-up N/A (pas de HDD).
**Onduleur : 10 kVA mini** pour ce seul cluster (voir ton guide
onduleurs : dimensionnement 1,5-2×, autonomie selon besoin).
Câblage : 2 PDU par rack (A/B), chaque nœud en double alimentation
répartie A/B.

## 190. Onduleur et séquence d'arrêt

Le stockage est **le dernier à s'éteindre et le premier à redémarrer** :
1. Arrêt des VM/applis (Proxmox),
2. Arrêt des clients Ceph,
3. `ceph osd set noout` puis arrêt des OSD/nœuds,
4. Arrêt des JBOD,
5. Arrêt onduleur.
Redémarrage inverse. **Automatise avec NUT** (voir ton `proxmox_guide.md`,
section NUT) et **teste la séquence** 2×/an. Un arrêt brutal répété,
c'est des OSD à reconstruire et des SSD sans power-loss qui corrompent.

## 191. NUT : le lien avec ton guide onduleurs

NUT (Network UPS Tools) : l'onduleur signale la coupure, le serveur
maître NUT ordonne l'arrêt séquentiel. Pour un cluster Ceph : le
délai d'arrêt d'un nœud avec 12 OSD peut dépasser 10 min (flush) —
règle l'autonomie et les temporisations NUT en conséquence, pas avec
les valeurs par défaut d'un serveur unique.

## 192. Rendement Titanium : le calcul (vérifié)

Alimentations Titanium : **96 %** à 50 % de charge (vérifié : specs
SSG-6049P). Sur un nœud 940 W : perte 38 W vs 75 W en Gold (92 %).
Pour 10 nœuds, 24/7 : (75-38) × 10 × 8760 h ≈ **3 240 kWh/an**
économisés, soit ~650 €/an à 0,20 €/kWh — sans compter la clim
évitée (~×1,5 avec PUE). L'écart de prix Gold→Titanium se rembourse
généralement en < 2 ans.

## 193. C-states, ASPM, power caps

- Active les C-states CPU et l'ASPM PCIe en BIOS : -5 à -15 % sur un
  nœud peu chargé, sans impact mesurable sur Ceph (à valider en bench).
- `powercap` RAPL : plafonne un nœud à X W en cas de contrainte
  électrique (canicule, groupe en maintenance).
- Sur NVMe : les états de puissance (5 états sur D7-PS1010, vérifié)
  se pilotent via `nvme set-feature` — à réserver aux nœuds peu
  sollicités (froid), jamais sur du chemin critique latence.

## 194. Refroidissement liquide des SSD (vérifié)

Deux réalités 2026 (vérifié 27/09/2026) : KIOXIA NX1 (E1.S, liquide
direct) et Solidigm D7-PS1010 E1.S (cold plate, co-développé avec
NVIDIA pour serveurs GPU fanless). Si tu densifies en E1.S Gen5/Gen6 :
**la boucle d'eau n'est plus optionnelle**, elle fait partie du design
mécanique du rack. Prévois les CDU et les débits avec ton équipe
énergie — c'est ton terrain.

## 195. Températures relevées (vérifié)

Test TweakTown (vérifié) : KIOXIA CD9P-R 7,68 To E3.S à **53 °C** en
écriture séquentielle soutenue avec refroidissement à air classique
(contre ~70 °C redoutés). Traduction : l'E3.S bien ventilé tient l'air ;
l'E1.S dense et le Gen6 demanderont plus. Sonde les SSD (`nvme smart-log`,
`temperature`) comme les HDD : alerte à 65 °C, critique à 75 °C.

## 196. Airflow : EDSFF vs U.2

L'argument thermique n°1 de l'EDSFF : les règles verticales laissent
**passer l'air entre elles**, là où un mur de 24 U.2 15 mm en façade
le bloque. Sur un 2U32 E3.S, le flux traverse le châssis ; sur un
vieux 2U24 U.2, il contourne. À densité égale, l'EDSFF demande moins
de pression statique → ventilateurs moins rapides → **moins de watts
de ventilation et moins de bruit**.

## 197. Coût énergie : €/an par To (exemple)

Nœud 60 baies : 980 W au mur × 8760 h = 8 585 kWh/an. Avec PUE 1,5 :
12 877 kWh/an. À 0,20 €/kWh : **~2 575 €/an** pour ~870 To utiles
(EC 8+3 sur 60×20 To) → **~3 €/To/an** d'énergie. Nœud NVMe : 800 W →
~2 100 €/an pour 25 To utiles → **~84 €/To/an**. L'énergie creuse
l'écart HDD/NVMe : intègre-la au TCO 5 ans, pas seulement le CAPEX.

## 198. Dimensionnement clim : exemple rangée

10 nœuds 60 baies = 9,8 kW IT → ~15 kW à climatiser (PUE 1,5).
En eau glacée : ~15 kW / (4,18 × ΔT 6 °C) ≈ 0,6 L/s. En détente directe :
vérifie la capacité des CRAC **par rangée**, pas globale — une rangée
stockage dense peut saturer son CRAC alors que la salle est à 50 %.
Voir ton `proxmox_guide.md` (dimensionnement) et `onduleurs_ups_guide.md`.

## 199. Redondance N+1 électrique

Double alimentation par nœud (A/B), 2 PDU par rack, 2 onduleurs ou
2 chaînes. Règle de capacité : **chaque chaîne doit tenir 100 % de la
charge** (pas 50 %) — sinon la perte d'une chaîne = surcharge de
l'autre = cascade. Teste le basculement en charge réelle 1×/an.

## 200. Le pic de spin-up : le chiffre

60 HDD × ~20 W au spin-up (2× le nominal, ~10-20 s) = **1 200 W**
transitoires par nœud. Sur 10 nœuds qui redémarrent ensemble après
une coupure : 12 kW de pic. Mitigations : spin-up **échelonné**
(staggered spin-up, réglage HBA/BIOS), redémarrage séquentiel des
nœuds, onduleur dimensionné pour le pic pas la moyenne.

## 201. Tableau récapitulatif énergie

| Architecture | W/To utile (ordre de grandeur) | PUE cible | Point chaud |
|---|---|---|---|
| HDD 60 baies EC | ~1-1,5 W/To | 1,4-1,6 | spin-up, airflow |
| Hybride | ~3-5 W/To | 1,4-1,6 | db SSD |
| Full NVMe réplica 3 | ~30-40 W/To | 1,3-1,5 | CPU/NIC, SSD Gen5 |

## 202. Checklist énergie

- [ ] Bilan de puissance par nœud (tableau 184/185) **écrit**
- [ ] Alimentations Titanium sur tout nœud neuf
- [ ] PDU A/B, chaque chaîne à 100 % de la charge
- [ ] Pic de spin-up intégré à l'onduleur
- [ ] Séquence d'arrêt NUT écrite et testée 2×/an
- [ ] Sondes température disques → supervision
- [ ] TCO 5 ans avec énergie, pas CAPEX seul

---

# PARTIE M — À VENIR : ANNONCES VÉRIFIÉES AU 27/09/2026

## 203. CXL : l'état réel (vérifié)

Compute Express Link : mémoire extensible et partageable sur PCIe.
État vérifié le 27/09/2026 (analyses 2026) :
- **CXL 2.0** : en production (ex. Azure), expansion mémoire Type 3.
- **CXL 3.x** : spécifications publiées, silicium et plateformes en
  cours de maturation — **pas de déploiement large** en 2026.
- Produits : Samsung CMM-D (E3.S), SK hynix CMM-DDR5 (E3.S), Micron
  CZ120/CZ122 (E3.S) — 96-256 Go, PCIe Gen5 x8.
Lien stockage : les modules CXL utilisent le **format E3.S** — tes
baies EDSFF de 2026 pourront accueillir de la mémoire CXL demain.
Pour le stockage pur, l'impact 2026-2027 reste indirect (caches
métadonnées géants, déduplication en mémoire).

## 204. Marvell Structera S 30260 (vérifié)

Switch CXL 3.0 : 260 voies, jusqu'à 4 To/s agrégés, **sampling Q3 2026**
(communiqué Marvell, vérifié). Le CXL 2.0 (Structera S 20256) est en
production. À suivre pour le pooling mémoire rack-scale 2027+.

## 205. EDSFF : la trajectoire (vérifié)

E3.S est en 2026 **le format standard du neuf** all-flash (Supermicro,
KIOXIA, Solidigm, Samsung, Micron). Prochaines étapes : E3.L pour les
très hautes capacités, Gen6 (Micron 9650 déjà là), et convergence avec
CXL (même connecteur E3.S). Acheter E3.S en 2026, c'est acheter le
format des 5 prochaines années.

## 206. PLC : l'état (vérifié)

PLC (5 bits/cellule) : Solidigm a démontré un **SSD PLC fonctionnel en
2022** (FMS), mais en 2026 **aucun produit PLC commercial** — Solidigm
déclare publiquement rester focalisée sur la QLC (interview TechRadar,
vérifiée) : « PLC faces major technical hurdles in endurance ».
Conclusion : **PLC = non trouvé en produit au 27/09/2026**. La densité
2026-2027 viendra de la QLC + 200+ couches, pas de la PLC.

## 207. SSD 200+ To : les annonces (vérifié)

- **Samsung BM1773 : 245,76 To**, QLC, PCIe 5.0, annoncé sept. 2026
  (vérifié) — pour datacenters IA haute densité, 0,6 DWPD.
- **Solidigm : roadmap 245 To+ avant fin 2026** (interview vérifiée).
- **KIOXIA LC9 : 245,76 To** E3.S (tableau sectoriel — **à vérifier**
  la disponibilité).
- **Micron 6600 ION : 122-245 To** E3.S, en qualification hyperscalers
  (tableau sectoriel — **à vérifier**).
- **SK hynix PS1012 : 244 To** (roadmap — **à vérifier**).
Avec des 245 To, un 2U32 E3.S = **7,8 Po bruts**. Le pétaoctet par U
devient le pétaoctet par **demi-U**. L'enjeu se déplace : rebuild,
blast radius, et surtout **le réseau**.

## 208. PCIe Gen6 / Gen7

Gen6 : 64 GT/s (~28 Go/s en x4) — le Micron 9650 est le premier SSD
Gen6 (vérifié, août 2026). Gen7 : spécification en cours, produits
**non trouvés au 27/09/2026**. Note : à partir de Gen6, un seul SSD
sature presque un lien 200G — le design réseau doit suivre (400G/800G).

## 209. HDD : HAMR et 30 To+ (vérifié)

- **Seagate Exos M 30 To** (HAMR/Mozaic 3+, 10 plateaux de 3 To) :
  ~565-600 $, soit **~19 $/To** (vérifié, sept. 2026) — le meilleur
  €/To du moment en entreprise, CMR, 7200 tr/min, 300 Mo/s, 5 ans
  de garantie, MTBF 2,5 M h.
- WD : gamme Ultrastar 24-28 To (à vérifier les références exactes).
- Trajectoire : 40-50 To HAMR annoncés en roadmap (à vérifier).
Le HDD n'est pas mort : à ~19 $/To vs ~350-600 $/To le NVMe, il reste
**20× moins cher** au To pour le froid. Le tiering HDD/QLC/NVMe a
encore de beaux jours.

## 210. QLC : la position officielle Solidigm (vérifié)

Interview Solidigm (vérifiée, 2026) : 120+ exaoctets de QLC livrés,
D5-P5336 122,88 To en production, focus QLC (pas PLC). La QLC est
devenue **la NAND du capacitaire flash** : 30,72 To en E3.S à ~0,6
DWPD. Pour ton tiering : QLC = le nouveau « disque froid performant »
entre HDD et TLC.

## 211. NVMe FDP — Flexible Data Placement (vérifié)

Supporté par le KIOXIA NX1 (vérifié) et la spec OCP Datacenter NVMe
SSD 2.6 : l'hôte indique au SSD où placer les données (par flux),
réduisant l'amplification d'écriture. Intérêt direct : **endurance
et perfs soutenues** sur workloads mixtes (Ceph !). À suivre côté
support noyau Linux / Ceph.

## 212. Computational storage (mention)

SSD avec calcul embarqué (compression, filtrage) : standard en
gestation, produits de niche. **Non trouvé en déploiement large au
27/09/2026** — à surveiller pour le traitement près des données IA.

## 213. Marché : allocation et prix (vérifié, sept. 2026)

Fusion Worldwide Greensheet (vérifié) : **allocation contrainte, prix
en hausse** sur : SSD entreprise PCIe 5.0 / haute capacité, HBM,
DDR5 haute densité. Conséquences achats : devis datés, commandes
fermes tôt, double sourcing, stocks tampons. Le « meilleur prix »
d'aujourd'hui peut être indisponible demain.

## 214. Ce qui n'existe pas (encore) au 27/09/2026

- SSD PLC commercial : **non trouvé**.
- PCIe Gen7 en produit : **non trouvé**.
- CXL 3.x en déploiement large : **non trouvé** (sampling).
- JBOD 90+ baies en catalogue public généraliste : **non trouvé**
  (vérifié partiellement — à vérifier chez intégrateurs).
- Disques SAS 24G grand public/grand volume : rares et chers.

## 215. Veille : les sources à suivre

- SNIA / EDSFF (specs formats), CXL Consortium (specs),
- communiqués Samsung / Solidigm / KIOXIA / Micron (roadmaps NAND),
- docs.ceph.com (releases Reef/Squid/Tentacle),
- Broadcom / Microchip (fiches HBA),
- Fusion Worldwide Greensheet (tension marché, mensuel).

## 216. Recommandations d'achat 2026-2027

1. **Neuf performant** : E3.S Gen5 (CD9P-R, D7-PS1010, PM9D3) — vérifié.
2. **Neuf capacitaire** : HDD 20-30 To CMR (Exos M 30 To — vérifié).
3. **HBA** : 9500 tri-mode (12G) ; 9600 si besoin 24G réel.
4. **Châssis** : E3.S 1U16/2U32 pour l'all-flash ; 4U60 pour le froid.
5. **Évite** : U.2 pour du neuf dense (sauf contrainte existante),
   cache tiering Ceph, PLC (n'existe pas), RAID 5 sur gros disques.

---

# PARTIE N — PIÈGES TERRAIN (17 PIÈGES VÉCUS OU DOCUMENTÉS)

## 217. Piège n°1 : la HBA livrée en IR au lieu d'IT

Symptôme : Ceph/ZFS ne voit qu'un « volume » ou rien. Cause : firmware
MegaRAID au lieu de HBA. Solution : reflasher en IT **avant** toute mise
en production, et vérifier `storcli /c0 show` (personality). Coût de
l'oubli : reconstruire le nœud.

## 218. Piège n°2 : le câble SlimSAS au mauvais brochage

Symptôme : lien qui ne monte pas, ou SSD NVMe vu en SAS. Cause : câble
SFF-8654 câblé PCIe branché sur backplane SAS (ou l'inverse). Solution :
commander les câbles **par référence constructeur du châssis**, pas « un
SlimSAS générique ». Garde la doc de brochage dans le dossier du rack.

## 219. Piège n°3 : le slot PCIe bridé

Symptôme : HBA 16 voies qui ne débite que la moitié. Cause : slot x16
mécanique mais **x8 ou x4 électrique** (fréquent sur cartes mères
denses). Solution : vérifier `lspci -vv | grep LnkSta` (vitesse **et**
largeur négociées) à l'installation. Un `x8` au lieu de `x16` sur une
9670 = 50 % du débit en moins.

## 220. Piège n°4 : le RAID 5 sur disques de 20 To

Symptôme : rebuild qui échoue à 80 %. Cause : URE pendant le rebuild
(section 91). Solution : RAID 6 minimum sur gros HDD, et au-delà d'une
baie : erasure coding logiciel. Ce piège a coûté des volumes entiers :
**ne transige pas**.

## 221. Piège n°5 : le block.db sous-dimensionné

Symptôme : OSD en erreur avec des HDD à 50 % (section 141). Cause : db
à 2,5 % sur un workload RGW qui demandait 4 %+. Solution : dimensionne
large dès le départ (4 % pour RGW), monitor l'espace db. Il n'y a pas
de « petit rajout » : c'est une recréation d'OSD.

## 222. Piège n°6 : le SSD QLC comme block.db

Symptôme : latences d'écriture qui explosent par vagues. Cause : QLC à
0,5 DWPD dont le cache SLC sature sous le journal Ceph. Solution :
**TLC 3 DWPD minimum** pour les db/wal (section 170). Le QLC reste sur
le froid.

## 223. Piège n°7 : le spin-down sur Ceph actif

Symptôme : latences aléatoires de plusieurs secondes. Cause : les
disques s'endorment et Ceph les réveille en permanence (scrub,
heartbeat). Solution : **désactive le spin-down** sur tout cluster
Ceph/ZFS actif ; réserve-le à l'archivage WORM pur (section 186).

## 224. Piège n°8 : le JBOD sans multipath

Symptôme : corruption après un flap de câble. Cause : dual-path SAS
sans `multipathd` → le même disque vu deux fois → écritures concurrentes.
Solution : multipath configuré **et testé** (débranche un câble en
burn-in) avant production (section 36).

## 225. Piège n°9 : l'expander en cascade triple

Symptôme : rebuild 3× plus lent que prévu. Cause : 3 JBOD en cascade
sur 8 voies = bande passante divisée + latence cumulée. Solution :
**2 niveaux de cascade max**, et de préférence une HBA par JBOD en
production (section 76).

## 226. Piège n°10 : le firmware flashé un vendredi

Symptôme : 60 disques qui disparaissent après un flash d'expander.
Cause : flash en production sans fenêtre, sans rollback. Solution :
firmware par vagues (1 nœud pilote → 48 h → reste), en fenêtre de
maintenance, avec l'ancien firmware sous la main (sections 37, 85, 173).

## 227. Piège n°11 : le PG count oublié

Symptôme : cluster à 30 % avec des OSD à 95 % (hot spots). Cause :
pool créé sans `bulk: true`, autoscaler jamais convergé, 32 PG pour
96 OSD. Solution : `bulk: true` sur les gros pools, vérifie
`ceph osd pool autoscale-status`, vise 100-200 PG/OSD (sections 108-110).

## 228. Piège n°12 : le réseau client/cluster mélangé

Symptôme : les VM rament pendant chaque recovery. Cause : un seul
réseau 25G pour clients + réplication. Solution : **sépare** (VLAN
mini, physique idéalement), 100G sur le cluster en full NVMe
(sections 114, 118).

## 229. Piège n°13 : le remplissage à 95 %

Symptôme : Ceph passe en `nearfull` puis `full`, écritures bloquées.
Cause : pas de marge (on a « optimisé » le dimensionnement). Solution :
**alerte à 70 %, critique à 85 %**, et achète l'extension à 70 %
(délais d'allocation 2026 : plusieurs semaines — section 213).

## 230. Piège n°14 : le disque SMR dans le RAID

Symptôme : rebuild à 5 Mo/s pendant des jours. Cause : SMR shingled
(section 103). Solution : **CMR uniquement** en datacenter ; vérifie
la fiche disque (les gammes grand public mélangent CMR/SMR sans le
crier).

## 231. Piège n°15 : l'arrêt électrique non séquencé

Symptôme : après une coupure, des OSD à reconstruire et des SSD
corrompus. Cause : extinction brutale, pas de séquence NUT (section
190). Solution : séquence écrite, **testée 2×/an**, onduleur
dimensionné pour le pic de spin-up (section 200).

## 232. Piège n°16 : le SSD grand public sans power-loss protection

Symptôme : filesystem corrompu après micro-coupure. Cause : SSD client
sans condensateurs de protection (PLP). Solution : en datacenter,
**SSD entreprise avec PLP** (tous les modèles cités en section 70
l'ont — vérifié). Le « bon plan » SSD gaming pour un serveur, c'est
une corruption en attente.

## 233. Piège n°17 : le prix au To brut

Symptôme : le « moins cher » coûte 2× plus cher à 5 ans. Cause :
comparaison en €/To brut sans énergie, sans surcoût de protection,
sans maintenance. Solution : **TCO 5 ans en €/To utile** incluant
kWh (PUE), RMA, et extensibilité (sections 121, 197). Fais signer ce
tableau avant l'achat : il évite 90 % des regrets.

---

# PARTIE O — GLOSSAIRE (40 TERMES)

## 234. Glossaire

| Terme | Définition |
|---|---|
| AFR | Annualized Failure Rate : % de disques morts par an. |
| Backfill | Recopie des PG sur les OSD survivants après une panne (Ceph). |
| BER | Bit Error Rate : taux d'erreur de lecture (10⁻¹⁴ à 10⁻¹⁷). |
| Block.db / block.wal | Métadonnées RocksDB et journal de BlueStore (Ceph). |
| BlueStore | Backend de stockage Ceph (remplace Filestore, obsolète). |
| Burn-in | Tests intensifs 48-72 h avant mise en production. |
| Cache tiering | Pool SSD devant pool HDD (déprécié dans Ceph). |
| CRUSH | Algorithme de placement pseudo-aléatoire des données (Ceph). |
| CXL | Compute Express Link : mémoire extensible sur PCIe. |
| DataBolt2 | Techno Broadcom : 24G amont avec disques 12G/6G aval. |
| Device class | Classe CRUSH (hdd, ssd, nvme) pour tiering par règles. |
| DWPD | Drive Writes Per Day : endurance normalisée sur 5 ans. |
| EC (Erasure Coding) | k fragments + m parités (Reed-Solomon), surcoût m/(k+m). |
| EDSFF | Enterprise & Datacenter Standard Form Factor (E1.S/E1.L/E3.S/E3.L). |
| EEDP / DIF | Protection des données de bout en bout (blocs 520 B). |
| Expander | « Switch » SAS : 1 port amont, N ports disques. |
| FDP | Flexible Data Placement : l'hôte guide le placement NVMe. |
| HAMR | Heat-Assisted Magnetic Recording : HDD 30 To+ (Seagate). |
| HBA | Host Bus Adapter : expose les disques bruts (mode IT). |
| IR mode | Firmware RAID intégré (MegaRAID) — pas pour Ceph/ZFS. |
| IT mode | Firmware JBOD pur — le mode Ceph/ZFS. |
| JBOD | Châssis de disques sans CPU, piloté par HBA externe. |
| LACP | Agrégation de liens Ethernet (802.3ad). |
| mClock | Ordonnanceur QoS de Ceph (client vs recovery). |
| MTBF | Mean Time Between Failures (ex. 2,5 M h sur Exos). |
| Multipath | Un seul device logique pour plusieurs chemins physiques. |
| NVMe-oF | NVMe over Fabrics : NVMe sur réseau (Ethernet/IB). |
| OSD | Object Storage Daemon : 1 par disque (Ceph). |
| Over-provisioning | NAND cachée au-delà de la capacité annoncée. |
| PG | Placement Group : unité de placement/réplication (Ceph). |
| PLC | Penta-Level Cell (5 bits/cellule) — pas de produit en 2026. |
| PLP | Power Loss Protection : condensateurs anti-coupure (SSD). |
| PUE | Power Usage Effectiveness : énergie totale / énergie IT. |
| QLC | Quad-Level Cell (4 bits/cellule) : capacitaire, ~0,5-1 DWPD. |
| RPO / RTO | Perte de données max / durée de reprise max acceptées. |
| SES | SCSI Enclosure Services : monitoring du JBOD. |
| SFF-8654 | Connecteur SlimSAS 24G (interne). |
| SFF-8674 | Connecteur SAS 24G (externe). |
| SMART | Self-Monitoring, Analysis and Reporting Technology (disques). |
| TBW / PBW | Tera/PetaBytes Written : endurance totale garantie. |
| TLC | Triple-Level Cell (3 bits/cellule) : le standard datacenter. |
| Tri-mode | SAS + SATA + NVMe sur le même contrôleur/baie. |
| UBM | Universal Bay Management : négociation tri-mode (SFF-TA-1005). |
| URE | Unrecoverable Read Error : erreur de lecture irrécupérable. |

---

# PARTIE P — QUIZ

## 235. Quiz : 10 questions

1. Quelle est la différence entre IT mode et IR mode, et lequel faut-il pour Ceph ?
2. Un lien SAS-4 « 24G » négocie en pratique à quel débit effectif par voie ?
3. Pourquoi le RAID 5 est-il déconseillé sur des disques de 20 To ?
4. Calcule le nombre de PG pour 96 OSD en réplica 3 (cible 100 PG/OSD).
5. Quelle taille minimale de block.db pour 40 HDD de 20 To en RGW ?
6. Combien d'OSD HDD au maximum par NVMe de block.db ?
7. Un nœud 60 baies HDD tire ~980 W au mur. Quelle puissance à climatiser avec un PUE de 1,5 ?
8. Pourquoi le spin-down est-il contre-productif sur un cluster Ceph actif ?
9. Quelle est la différence physique entre U.2 et U.3 ?
10. Cite deux raisons de préférer l'erasure coding 8+3 au réplica 3 pour de l'objet froid.

## 236. Quiz : réponses

1. IT = JBOD pur (disques bruts) ; IR = RAID matériel intégré. Pour Ceph/ZFS : **IT toujours** (le logiciel gère redondance et checksums).
2. **22,5 Gb/s** effectifs par voie (24G après encodage — vérifié sur fiches 9600).
3. Risque d'**URE pendant le rebuild** (BER 10⁻¹⁴ sur 20 To lus) → rebuild qui échoue → perte du volume ; + write hole.
4. (100 × 96) / 3 = 3200 → **4096** (puissance de 2) → 128 PG/OSD, dans la fourchette 100-200.
5. 40 × 20 To × 4 % (RGW) = **32 To** de block.db.
6. **≤ 15** OSD HDD par NVMe (4-5 par SSD SATA).
7. 980 W × 1,5 = **~1 470 W** à climatiser.
8. Ceph/ZFS réveillent les disques en permanence (scrub, heartbeat) → cycles incessants = latence + usure mécanique.
9. **Même connecteur physique** ; U.3 ajoute la négociation **tri-mode** (SAS/SATA/NVMe) via UBM côté backplane.
10. Surcoût **37,5 % vs 200 %** (donc ~2× moins de To bruts à acheter) et tolérance de **3 pannes distribuées** sur le cluster.

---

**Fin du guide — vérifications web au 27/09/2026. Prix et disponibilités : allocation tendue, devis datés exigés.**

---

# PARTIE Q — CAS CHIFFRÉS DÉTAILLÉS (RETOURS TERRAIN)

## 237. Cas n°1 : backup 1 Po utile (froid, EC)

Hypothèses : 1 Po utile (1 000 To), backup Veeam/PBS, fenêtre 8 h,
rétention GFS. Protection : EC 8+3 (37,5 %). Disques : Exos M 30 To
(vérifié : ~19 $/To, 27/09/2026).
Brut = 1000 × 1,375 × 1,15 (marge) ≈ 1 581 To → **54 HDD de 30 To**
(1 620 To bruts). Nœuds : 6 nœuds × 9 HDD (k+m = 11 ≤ 6 nœuds ? Non !
EC 8+3 exige au moins 11 domaines : **12 nœuds × 5 HDD** ou 6 nœuds
avec 2 racks/failure-domains — voir piège 240). Prenons **11 nœuds ×
5 HDD** = 55 disques. DB : 55 × 30 To × 2,5 % = 41 To → 6 NVMe 7,68 To
(1 par nœud + 1 spare). Réseau : 2×25G/nœud. Fenêtre backup : 1 Po /
8 h = 35 Go/s — **irréaliste** en 25G : revois la fenêtre (incrémentiel
quotidien ~5 % = 50 To = 1,8 Go/s OK) ou monte en 100G.

## 238. Cas n°1 (suite) : TCO 5 ans

| Poste | Calcul | Montant |
|---|---|---|
| 55 HDD 30 To × ~570 € | 31 350 € | 31 k€ |
| 6 NVMe 7,68 To × ~3 500 € | 21 000 € | 21 k€ |
| 11 nœuds (châssis+CPU+RAM) | 11 × 4 000 € | 44 k€ |
| Réseau 25G (switchs + NIC) | — | ~15 k€ |
| CAPEX total | | **~111 k€** |
| Énergie : 11 × 400 W × 8760 × 1,5 × 0,20 € | | ~11,6 k€/an → 58 k€/5 ans |
| **TCO 5 ans** | | **~170 k€ → ~170 €/To utile** |

Ordres de grandeur (**à vérifier** sur devis). L'énergie = 1/3 du TCO :
le HDD « pas cher » se paie en kWh.

## 239. Cas n°2 : cluster IA training (débit extrême)

Hypothèses : nourrir 8 GPU, besoin 200 Go/s lecture séquentielle,
dataset 500 To. Disques : NVMe Gen5 E3.S 15,36 To (7 Go/s soutenables
chacun en lecture). Disques = 200 / 7 ≈ 29 → **32 NVMe** (marge).
Nœuds : 2 nœuds × 16 NVMe ? Non : blast radius + PCIe. **4 nœuds ×
8 NVMe** (Gen5 x4 = 8×16 = 128 voies PCIe par nœud — vérifie le
bilan PCIe de la carte mère !). Réseau : 200 Go/s = 1,6 Tb/s →
**4× 400G par nœud** ou NVMe-oF direct. Protection : réplica 2
(données reproductibles depuis l'objet froid) ou EC 4+2 local.
Alternative : **WEKA/Lustre** (section 157-158) — à chiffrer contre
Ceph, car à ce débit le tuning Ceph est un projet en soi.

## 240. Piège du cas n°1 : k+m vs nombre de nœuds

EC 8+3 = 11 fragments sur 11 **domaines de panne** distincts. Avec
6 nœuds, CRUSH ne peut pas placer 11 fragments sur 6 hosts sans en
mettre 2 sur le même host → perte d'un host = 2 fragments perdus,
il ne reste que 9 ≥ 8 (k) : ça tient **mathématiquement** mais la
marge fond. Règle : **k+m ≤ nombre de failure domains**, ou accepte
explicitement la dégradation. D'où 11 nœuds au cas n°1 — ou EC 4+2
(6 domains) sur 6 nœuds.

## 241. Cas n°3 : PME, Proxmox + Ceph HCI (3 nœuds)

Hypothèses : 30 To utiles VM, 3 nœuds Proxmox+Ceph convergés (lien
avec ton `proxmox_guide.md`). Disques : 6 NVMe 7,68 To par nœud =
18 OSD, réplica 3. Brut = 18 × 6,99 = 125,8 Tio → utiles ≈ 125,8 /
3 × 0,8 ≈ 33 To. RAM : 6 × 3 + 16 + 32 (VM) = **128 Go/nœud**.
CPU : 2× 12c. Réseau : 2×25G (client+cluster sur VLAN séparés —
compromis acceptable à cette échelle). PG : (100×18)/3 = 600 →
**512**. Coût ordre de grandeur : 3 × ~15 k€ = **~45 k€** (à vérifier).
C'est le sweet spot PME : simple, résilient, évolutif par ajout de
nœuds.

## 242. Cas n°4 : objet S3 2 Po (froid, QLC)

Hypothèses : 2 Po utiles S3, RGW, EC 8+3. Disques : SSD QLC E3.S
30,72 To (D5-P5430, vérifié : 0,58 DWPD — OK pour du WORM froid).
Brut = 2000 × 1,375 × 1,15 ≈ 3 162 To → **104 SSD de 30,72 To**.
Nœuds : 13 nœuds × 8 SSD (13 ≥ 11 domains). DB : RGW → 4 % :
104 × 30,72 × 4 % = 128 To de db → les SSD étant homogènes, db
colocalisé (full flash). RAM : RGW gourmand en omap → **256 Go/nœud**.
Conso : 13 × 500 W ≈ 6,5 kW IT. CAPEX disques : 104 × 30,72 To ×
~250 €/To (QLC moins cher — à vérifier) ≈ **800 k€**. Le QLC fait
du 2 Po « performant-froid » là où le HDD demanderait 3× plus de
baies.

## 243. Comparaison des 4 cas (€/To utile/an, TCO 5 ans)

| Cas | €/To utile (TCO 5 ans) | W/To | Latence |
|---|---|---|---|
| Backup 1 Po HDD EC | ~170 € | ~1 W | ~10 ms |
| IA training NVMe | à vérifier (élevé) | ~20 W | < 0,5 ms |
| PME 30 To HCI | ~300 € (ordre de grandeur) | ~15 W | < 2 ms |
| S3 2 Po QLC EC | ~500 € (ordre de grandeur) | ~4 W | ~2 ms |

Tous **à vérifier** sur devis — mais les **rapports** entre cas sont
robustes : le froid HDD coûte ~10× moins cher au To que le NVMe
performant.

## 244. Le dimensionnement PCIe : l'oublié

Un nœud 24 NVMe Gen5 x4 = 96 voies PCIe rien que pour les disques +
2×100G (32 voies) + HBA. Total > 128 voies : il faut du **double
socket** ou des CPU à 128+ voies (EPYC). Vérifie le **bilan PCIe**
de la carte mère **avant** d'acheter les disques : un SSD branché
sur un lien x2 au lieu de x4 perd la moitié de son débit (même
famille de piège que la section 219).

## 245. Le dimensionnement mémoire du MON/MGR

Sur un gros cluster (500+ OSD), les MON deviennent gourmands :
**32-64 Go** par MON, SSD dédié pour la base (les MON écrivent peu
mais lisent beaucoup). 5 MON pour > 200 OSD. Ne colle jamais un MON
sur un nœud OSD saturé en production — ou accepte des élections
lentes en cas de panne.

## 246. RGW : le dimensionnement des gateways

1 gateway RGW ≈ 10-20 Gb/s de S3 (selon TLS et taille d'objets).
Pour 100 Gb/s de S3 : **6-10 gateways** derrière un load-balancer,
sur des nœuds dédiés ou colocalisés (colocalisé = moins cher, dédié
= plus prévisible). Le TLS termine **sur les gateways** (pas sur le
LB) si tu veux du bout-en-bout — avec le coût CPU associé (~1 cœur
/ 5 Gb/s TLS, ordre de grandeur à vérifier).

## 247. Erasure coding et CPU : le chiffrage

Encodage EC 8+3 à 1 Go/s ≈ 2-4 cœurs modernes (isa-l). Sur un nœud
qui ingère 5 Go/s en EC : **10-20 cœurs** rien que pour l'encodage.
C'est pour ça que la section 112 prend la fourchette haute en RGW/EC.
Si le CPU est le goulot : passe en réplica ou ajoute des nœuds.

## 248. Le « petit » cluster : 3 nœuds, 36 HDD

Le classique qui échoue : 3 nœuds × 12 HDD 20 To, réplica 3, PG auto.
Problèmes : 36 OSD seulement → PG/OSD faibles si mal réglés ;
perte d'un nœud = 33 % de capacité en moins = **nearfull immédiat**
si le cluster était à 70 %. Règle : sur 3 nœuds, ne dépasse jamais
**50 % de remplissage** (sinon la perte d'un nœud tue le cluster).

## 249. Extension : ajouter des nœuds

Ajouter 2 nœuds à un cluster de 6 : +33 % de capacité, mais le
rebalance déplace ~1/4 des données → **trafic cluster massif**
pendant des heures/jours. Procédure : `ceph osd set norebalance`
pendant les heures ouvrées, ajout la nuit, `unset` ensuite, et
surveille le `misplaced`. Préviens les utilisateurs : la latence
p99 bouge pendant un rebalance.

## 250. Retrait : sortir un nœud

`ceph osd out` nœud par nœud, **jamais** `ceph osd purge` brutalement
en production. Le drain d'un nœud 12×20 To = 240 To à recopier :
à 5 Go/s = 13 h. Pendant ce temps, le cluster est dégradé : **un
seul nœud à la fois**, et pas pendant la fenêtre de backup.

## 251. Le multi-site : stretch cluster

Ceph stretch (2 sites + tiebreaker) : survit à la perte d'un site.
Coût : latence d'écriture = RTT inter-sites (2 ms à 50 km = OK pour
du backup, KO pour de l'OLTP synchrone). Alternative : 2 clusters +
réplication asynchrone (RGW multisite). Pour ton métier : le stretch
impose aussi le **lien énergie** des deux sites (2 onduleurs, 2 groupes).

## 252. Benchmark : la méthode honnête

1. Préconditionne les SSD (2× écriture complète).
2. `rados bench` sur pool **dédié** (jamais prod — supprime les objets
   `bench*` après, cause documentée de OSD_FULL).
3. Teste 4K aléatoire QD32, 128K séquentiel, 70/30 mixte.
4. Mesure **p99**, pas la moyenne.
5. Refais après remplissage à 50 % (les perfs changent avec le
   remplissage sur QLC et sur HDD zone externe/interne).

## 253. HDD : zones et débit variable

Un HDD 20 To ne fait pas 280 Mo/s partout : ~280 Mo/s en zone externe,
~150 Mo/s en zone interne. En début de vie (vide), tout est rapide ;
à 80 % plein, les écritures tombent en zone lente. Pour les SLA de
backup : dimensionne sur le **débit zone interne**, pas le pic.

## 254. SSD : le steady state

Un SSD neuf sort de boîte 2× plus vite qu'après préconditionnement.
Tous les chiffres constructeur sont en **burst**. Pour dimensionner :
utilise les chiffres **steady state** (tests avec préconditionnement —
vérifié : méthodologie TweakTown). Écart typique : 30-50 % de moins
en écriture soutenue sur TLC, 70 % sur QLC.

## 255. Le test de panne : game day

1×/trimestre : tue un OSD (en prod, en heures creuses), mesure le
temps de recovery et l'impact p99. 1×/an : tue un nœud entier. 1×/an :
coupure électrique simulée (NUT). **Documente** les résultats : c'est
ce document qui justifie le budget d'extension quand le DSI demande
« pourquoi il faut 30 % de marge ».

## 256. Capacité vs performance : le point de bascule

Formule : si `capacité_utile / débit_cible` < 1 h, tu es **limité en
capacité** (ajoute des disques) ; si > 10 h, tu es **limité en
performance** (ajoute du réseau/CPU). Entre les deux : les deux.
Exemple : 100 To utiles à 10 Go/s = 2,8 h → équilibré. À 100 Go/s =
0,28 h → le réseau/CPU domine, les disques sont presque « gratuits ».

## 257. Le tiering manuel : politique de déplacement

Sans cache tiering (section 137), le tiering = **politique** : données
< 30 jours sur NVMe (device class), > 30 jours migrées sur HDD/EC par
script applicatif (RGW lifecycle, rbd migration, cron + rsync).
C'est moins magique que du tiering automatique, mais c'est **prévisible**
et ça ne corrompt rien. Documente la politique, alerte sur son échec.

## 258. La doc d'exploitation minimale

Pour chaque cluster : schéma réseau + électrique, plan de câblage SAS
(photo), versions firmware (HBA/expander/SSD), seuils d'alerte,
procédures (ajout/retrait nœud, RMA disque, upgrade Ceph, arrêt NUT),
contacts support avec numéros de contrat. **Si ce n'est pas écrit,
ça n'existe pas** — surtout à 3 h du matin en astreinte.

---

# PARTIE R — RUNBOOKS D'EXPLOITATION

## 259. Runbook : remplacement d'un disque Ceph

1. `ceph osd out <id>` (le cluster recopie).
2. Attends `HEALTH_OK` (ou `active+clean` sur les PG concernées).
3. `ceph osd purge <id> --yes-i-really-mean-it` (note l'ID).
4. Remplace physiquement (LED locate via SES : `sg_ses --set=ident`).
5. `ceph-volume lvm create --data /dev/XXX` (recrée l'OSD).
6. Vérifie le crush (même host, même device class).
7. Burn-in du disque **retiré** avant de le mettre en spare.
Temps total : 10 min d'ops + heures de backfill. **Ne purge jamais
avant le `out` complet.**

## 260. Runbook : remplacement d'un disque RAID matériel

1. Identifie via `storcli /c0/eX/sY show` + LED.
2. `storcli /c0/eX/sY set offline` puis `set missing` si besoin.
3. Remplace à chaud.
4. Le rebuild démarre (auto ou `start rebuild`).
5. Surveille `%` et `storcli /c0 show rebuild`.
6. **Ne reboote pas** pendant le rebuild (sauf urgence).
Un rebuild interrompu repart de zéro sur beaucoup de cartes.

## 261. Runbook : upgrade Ceph (cephadm)

1. Sauvegarde la conf (`ceph config dump`) et vérifie les backups.
2. `ceph orch upgrade check --image ...`.
3. `ceph orch upgrade start --image ...` (par défaut : 1 daemon à
   la fois, avec `noout` géré).
4. Surveille `ceph orch upgrade status` + `ceph health`.
5. **Ne touche pas aux PG** pendant l'upgrade (autoscaler en pause,
   section 131).
6. Valide : `ceph versions`, bench rapide, puis rouvre les vannes.
Rollback : `ceph orch upgrade stop` — teste-le en lab d'abord.

## 262. Runbook : extension d'un JBOD

1. Câble le nouveau JBOD (domaines A/B, étiquettes).
2. Vérifie la découverte : `storcli /c0 show all` (nouveaux
   enclosures + disques).
3. Burn-in des nouveaux disques **avant** de les confier au cluster.
4. Ajoute au cluster (Ceph : `ceph orch daemon add osd` / `ceph-volume`).
5. Laisse le rebalance converger (norebalance en heures ouvrées).
6. Mets à jour le plan de câblage et la doc.

## 263. Runbook : panne HBA

Symptôme : tous les disques d'un domaine disparaissent d'un coup.
1. `lspci` : la carte est-elle visible ? Non → reseat, puis RMA.
2. Visible mais pas de disques → câble, puis expander, puis firmware.
3. Bascule sur le domaine B (multipath) le temps du diagnostic.
4. **Stock de rechange** : une HBA identique flashée en IT en spare
   (le flash prend 30 min que tu n'as pas en pleine panne).

## 264. Runbook : corruption suspectée (ZFS/Ceph)

1. `zpool scrub` / `ceph pg repair` sur le périmètre suspect.
2. Si erreurs persistantes : isole le disque (`smartctl`, `nvme
   error-log`), remplace.
3. Si erreurs sur plusieurs disques : suspecte **RAM** (memtest),
   HBA/câble, ou alimentation (tension instable = corruption).
4. Restaure depuis backup si la redondance est dépassée — d'où la
   règle : le RAID/Ceph n'est pas un backup (section 89).

## 265. Runbook : cluster Ceph en HEALTH_WARN

1. `ceph health detail` : lis **tout**, dans l'ordre.
2. `ceph -s` : OSD down ? PG degraded ? nearfull ?
3. `ceph osd tree` : localise (host ? rack ?).
4. Causes fréquentes : OSD down (disque/HBA), clock skew (chrony !),
   PG stuck (pg_num), nearfull (extension).
5. **Ne redémarre jamais tous les MON/OSD « pour voir »** : tu
   transformes un WARN en disaster.

## 266. Le clock skew : l'ennemi silencieux

Ceph exige des horloges synchronisées (MON quorum, heartbeats).
> 50 ms de décalage = WARN, > 1 s = OSD flapping. Cause n°1 :
**chrony mal configuré** ou VM sans horloge fiable. Vérifie
`chronyc tracking` sur **chaque** nœud après chaque reboot. Un cluster
qui flap « sans raison » a souvent une horloge qui dérive.

## 267. Supervision : quoi monitorer (Zabbix/Prometheus)

- `ceph health`, PG states, OSD down (via MGR/prometheus).
- SMART : attributs 5/187/197/198 (HDD), percentage_used (NVMe).
- Températures : disques, HBA (`storcli`), expander (SES).
- Réseau : erreurs CRC, paquets perdus (le RDMA n'aime pas la perte).
- Énergie : PDU par prise (dérive de conso = disque/ventilo qui meurt).
- Espace : 70 % warn, 85 % crit (section 229).
Voir ton `zabbix_guide.md` pour les templates.

## 268. Les logs à garder

- `ceph log` / `ceph audit log` : 90 jours mini.
- SMART de référence post burn-in : **à vie du disque**.
- Versions firmware par série : à vie du parc.
- Historique des pannes par modèle : le tableau qui justifie les
  bannissements (section 177).
En cas de litige garantie : le log fait foi, pas ta mémoire.

## 269. RMA : la discipline

1. Photo du label (série) avant démontage.
2. Secure erase si possible (section 305).
3. Emballage antistatique d'origine (un disque RMA mal emballé =
   refus de garantie).
4. Suivi : date d'envoi, n° RMA, délai. Un fournisseur qui dépasse
   15 jours ouvrés deux fois = renégocie le contrat ou change.
5. Le disque de remplacement subit un **burn-in** (neuf ou reconditionné).

## 270. Le stock de rechange : la formule

`spare = max(2, 5 % du parc)` par modèle de disque, + 1 HBA + câbles
de chaque type + 1 alimentation par modèle. Pour 600 HDD : 30 disques
en spare (~15 k€) contre une indisponibilité. Le spare se **teste**
(burn-in) et se **renouvelle** (FIFO : le plus vieux part en premier).

---

# PARTIE S — RÉSEAU DE STOCKAGE APPROFONDI

## 271. iSCSI : le bloc sur Ethernet

iSCSI = SCSI sur TCP/IP. Simple, partout, 10/25/100G. Latence : ~0,5-1 ms.
Usage : connecter des serveurs à une baie ou un cluster sans Fibre Channel.
Limites : le TCP ajoute de la latence vs FC/NVMe-oF ; le multipath iSCSI
se configure (2 sous-réseaux, pas de LACP sur une session). En 2026 :
correct pour la PME, dépassé pour le NVMe performant.

## 272. Fibre Channel : le roi historique du SAN

16/32/64G FC, latence ~0,2-0,5 ms, lossless par construction, zoning
matériel. Toujours présent dans les SAN entreprise classiques. Mais :
cartes + switchs propriétaires et chers, compétences rares, et le
NVMe-oF sur Ethernet grignote son terrain. Pour un **nouveau** projet
2026 : ne choisis le FC que si l'existant l'impose.

## 273. NVMe-oF : le NVMe sur le réseau

NVMe over Fabrics : le protocole NVMe encapsulé sur TCP, RDMA (RoCE/IB)
ou FC. Latence ajoutée : ~5-10 µs en RDMA — quasi du local. Usage :
désagréger le flash (baies NVMe partagées façon SAN moderne), ou
connecter des initiateurs à un cluster. En 2026 : de plus en plus
d'offres (baies EDSFF + NVMe-oF/TCP), support Linux mature
(`nvme-cli`, discovery).

## 274. NVMe/TCP vs NVMe/RDMA

| Transport | Latence ajoutée | Matériel | Complexité |
|---|---|---|---|
| NVMe/TCP | ~20-50 µs | Ethernet standard | faible |
| NVMe/RDMA (RoCE v2) | ~5-10 µs | NIC RDMA, lossless | élevée |

Pour 95 % des usages : **NVMe/TCP suffit** et s'opère comme de l'iSCSI.
Le RDMA se justifie quand chaque microseconde compte (trading, OLTP
extrême) — avec le coût ops du lossless Ethernet (PFC, ECN).

## 275. RoCE : le lossless à configurer

RDMA over Converged Ethernet exige un réseau **sans perte** : PFC
(Priority Flow Control) + ECN sur tous les switchs du chemin. Une
erreur de config PFC = deadlocks et perfs catastrophiques. Règle :
si tu n'as pas un admin réseau qui **maîtrise** le DCB, reste en
NVMe/TCP ou iSCSI. Le RDMA mal configuré est pire que le TCP.

## 276. Topologie réseau Ceph : le schéma complet

```
                    ┌─────────────┐
                    │ 2× switch   │
                    │ 100G (MLAG) │
                    └──────┬──────┘
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │ Nœud 1   │ │ Nœud 2   │ │ Nœud 3   │ ...
        │2×100G clu│ │2×100G clu│ │2×100G clu│  ← VLAN cluster
        │2×100G pub│ │2×100G pub│ │2×100G pub│  ← VLAN client
        └──────────┘ └──────────┘ └──────────┘
```
2 switchs en MLAG (pas de SPOF), LACP 2 liens par VLAN par nœud,
MTU 9000 (jumbo) sur le VLAN cluster **de bout en bout** (un seul
équipement en 1500 = fragmentation silencieuse).

## 277. MTU 9000 : le checklist

Jumbo frames : -10 à -20 % de CPU réseau, +débit. Mais : **tous** les
équipements du VLAN (NIC, switchs, BMC si partagé — ne partage pas),
et vérifié avec `ping -M do -s 8972`. Le classique : ça marche en
lab (1 switch) et ça casse en prod (un uplink oublié en 1500).

## 278. Bande passante : Ethernet 2026

| Débit | Usage stockage 2026 | Note |
|---|---|---|
| 25G | nœud HDD/hybride, client | le standard économique |
| 100G | nœud NVMe, cluster Ceph | le standard perf |
| 200/400G | gros nœuds 32 NVMe Gen5, IA | prix en baisse, à vérifier |
| 800G | inter-switch, backbone IA | early 2026 |

Règle : le réseau se choisit **après** le calcul de la section 113,
pas avant. Un cluster full NVMe en 25G, c'est une Ferrari avec des
roues de vélo.

## 279. Latence réseau : les ordres de grandeur

| Segment | Latence typique |
|---|---|
| NVMe local | 50-100 µs |
| NVMe-oF RDMA | +5-10 µs |
| iSCSI 25G | 0,5-1 ms |
| Ceph RBD réplica 3 (100G) | 0,5-2 ms |
| Ceph EC (encodage + fan-out) | 2-5 ms |
| Inter-sites 50 km | +0,5 ms (RTT) |

Si ton application exige < 1 ms : NVMe local ou NVMe-oF/RDMA, **pas**
Ceph réplica 3 (sauf cluster très bien réglé et peu chargé).

## 280. Le réseau et l'énergie

4 NIC 100G ≈ 80 W par nœud (section 185) + 2 switchs 32×100G à
~300 W chacun. Sur un cluster 8 nœuds : ~1,2 kW rien que pour le
réseau, soit ~15 % du budget. Les NIC 400G consomment plus (~30-40 W
par port, ordre de grandeur à vérifier) : le réseau du stockage IA
est un poste énergétique à part entière.

## 281. Erreurs réseau : les signes avant-coureurs

`ethtool -S` : CRC, dropped, fifo_errors. En stockage : **0 erreur
tolérée** en régime établi. Une erreur CRC de temps en temps =
câble/fibre/ transceiver à changer **maintenant**. Les problèmes
intermittents réseau donnent des symptômes « disque lent » (slow ops)
qui font accuser les SSD à tort : vérifie le réseau **avant** de RMA
un SSD.

## 282. Sécurisation du réseau stockage

VLAN dédiés, pas de routage vers Internet, ACL sur les switchs,
authentification Ceph (cephx) activée, et pour RGW : TLS partout.
Le réseau cluster Ceph en clair sur un VLAN dédié = acceptable ;
le même en clair sur le LAN bureautique = non.

---

# PARTIE T — SÉCURITÉ, CHIFFREMENT, FIN DE VIE

## 283. Le chiffrement : où et pourquoi

| Couche | Solution | Protège contre |
|---|---|---|
| Disque (SED/Opal) | chiffrement matériel | vol du disque |
| Ceph (dmcrypt/LUKS) | OSD chiffrés | vol du nœud |
| Réseau (TLS/IPsec) | RGW TLS, msgr v2 | écoute réseau |
| Application | chiffrement applicatif | tout (bout-en-bout) |

Le minimum datacenter 2026 : **SED ou LUKS** sur les disques +
TLS sur RGW. Le vol d'un JBOD de 60 disques non chiffrés, c'est
1,8 Po de données en clair sur leboncoin.

## 284. SED et TCG Opal (vérifié)

Les SSD entreprise cités (D7-PS1010, CD9P-R, D5-P5430) proposent
**TCG Opal 2.0** (vérifié 27/09/2026), certains en FIPS 140-2/3.
Le chiffrement est matériel (pas de perte de perfs), piloté par
`sedutil-cli`. Point d'attention : la **gestion des clés** (un SED
dont on perd la clé = une brique). Intègre les clés au coffre
(Vault) avec procédure de recouvrement.

## 285. LUKS sur OSD Ceph

`ceph-volume lvm create --dmcrypt` : chaque OSD chiffré. Coût CPU :
~5-15 % sur workloads chiffrés (AES-NI), plus sur petits IO. En
full NVMe : mesure avant/après — sur des CPU modernes le coût est
souvent acceptable. Avantage : pas de dépendance au firmware du
disque, clés centralisables.

## 286. Secure erase : la procédure

Avant RMA, revente ou réforme :
1. `nvme format --ses=2` (crypto erase, instantané sur SED) ou
   `hdparm --security-erase` (HDD),
2. vérifie (lecture de contrôle),
3. **certificat de destruction** pour les disques sensibles
   (dégaussage/broyage si secret défense — filière agréée).
Un disque « formaté » n'est pas un disque effacé : le formatage
rapide ne touche pas les blocs réalloués (SMART 5 !).

## 287. Blocs réalloués et données résiduelles

Les secteurs réalloués (attribut 5) sont **illisibles par l'OS**
mais physiquement présents sur les plateaux/NAND. Sur disque
sensible : crypto-erase (rend tout illisible via la clé) ou
destruction physique. C'est l'argument massue pour le **SED
systématique** : l'effacement = destruction de la clé.

## 288. RGW : Object Lock et immuabilité

S3 Object Lock (WORM) : l'arme anti-ransomware pour les backups
(section 161). Configure : retention compliance sur les buckets
de backup, **avec une durée supérieure à la rétention**. Teste la
restauration **avec** le lock (certains outils de backup ne
gèrent pas la suppression des versions verrouillées).

## 289. Snapshots vs backups

Un snapshot (Ceph RBD, ZFS) n'est **pas** un backup : il est sur le
même cluster (incendie, ransomware, `rbd snap purge` par erreur).
Règle 3-2-1 : 3 copies, 2 supports, 1 hors site. Pour Ceph : export
rbd vers un cluster distant, ou RGW avec réplication + Object Lock.

## 290. Contrôle d'accès Ceph

cephx : chaque client a une clé avec des **capabilities minimales**
(`mon 'profile rbd' osd 'profile rbd pool=vm'`). Ne distribue jamais
la clé admin. Pour RGW : IAM-like (users, buckets policies). Audite
les clés 1×/an : une clé d'ancien prestataire qui traîne, c'est
classique.

## 291. Audit et traçabilité

`ceph audit log` : qui a fait quoi (création pool, purge OSD...).
Envoie-le au SIEM. En cas d'incident : c'est la première source.
Conserve 1 an mini (contrainte réglementaire selon secteur).

---

# PARTIE U — TCO, ACHATS, GABARITS

## 292. TCO 5 ans : le gabarit

| Poste | Année 1 | Années 2-5/an |
|---|---|---|
| CAPEX matériel | X | — |
| Énergie (kWh × PUE × prix) | — | Y |
| Maintenance / support | — | Z |
| RMA / spares (5 % du parc) | — | W |
| Ops (heures × TJM interne) | — | V |
| Extension prévue (année 3 ?) | — | E |

`TCO = X + 5×(Y+Z+W+V) + E`. Compare les architectures sur ce
tableau, pas sur le CAPEX. Le NVMe perd sur le CAPEX et gagne sur
les ops (moins de baies, moins de pannes mécaniques) : le TCO
arbitre honnêtement.

## 293. L'appel d'offres : les clauses qui comptent

- Firmware **IT** imposé (HBA), UBM pour les baies tri-mode.
- Garantie 5 ans disques, avec **délai de remplacement écrit**
  (J+1 ? J+5 ?).
- SLA pièces : stock local ou expédition 48 h ?
- Droit au firmware sans contrat de support (certains OEM
  verrouillent les téléchargements).
- Pénalités de retard (allocation tendue 2026 — section 213).
- **Deux fournisseurs qualifiés** minimum par famille de disques.

## 294. Le double sourcing

Ne qualifie jamais un seul modèle de SSD/HDD : une rupture ou un
bug firmware par lot te laisse sans option. Deux modèles qualifiés
(même capacité, perfs proches) = négociation + résilience. En
pratique : 70/30 sur les volumes, avec le 30 % qui tourne vraiment
(pas juste « qualifié sur papier »).

## 295. Reconditionné : quand c'est pertinent

HDD Exos/Ultrastar reconditionnés garantis 1-2 ans : pertinents pour
du **froid non critique** ou du lab (prix ~50 % du neuf — à vérifier).
Jamais pour : block.db, cache, production critique sans spare
massif. Vérifie : heures de vol (SMART 9), `percentage_used` (SSD),
et bannis les modèles à AFR élevé (section 177).

## 296. Le planning type d'un projet

| Phase | Durée indicative |
|---|---|
| Dimensionnement + devis | 3-4 semaines |
| Commande (allocation 2026) | 4-12 semaines |
| Réception + burn-in | 2-3 semaines |
| Installation + câblage | 1-2 semaines |
| Tests (bench + game day) | 2 semaines |
| Production progressive | 2-4 semaines |

Total : **4-6 mois** de la décision à la production. Le burn-in et
les tests ne se compressent pas : c'est là que les projets « rapides »
meurent.

## 297. Gabarit BOM : nœud Ceph HDD standard

```
Nœud Ceph HDD (×N) :
- Châssis 2U 12 baies 3,5" / backplane expander 12G
- 1× CPU 16c, 128 Go DDR5 ECC
- 1× HBA 9500-16i (IT)
- 12× HDD 20 To CMR (burn-in OK)
- 1× NVMe 3,84 To TLC (block.db, 4 DWPD si RGW)
- 2× 25G NIC (1 pub + 1 cluster) ou 2×25G ×2
- 2× PSU Titanium
- Rails + obturateurs
```

## 298. Gabarit BOM : nœud Ceph NVMe standard

```
Nœud Ceph NVMe (×N) :
- Châssis 2U 16-24 baies E3.S Gen5 (NVMe direct si possible)
- 2× CPU 16c ou 1× 32c, 256 Go DDR5 ECC
- 16× NVMe 7,68 To Gen5 E3.S TLC 1 DWPD
- 2× 100G NIC client + 2× 100G NIC cluster
- 2× PSU Titanium 1600W
- Câblage DAC/fibre + 2 switchs 100G MLAG (mutualisé)
```

## 299. Gabarit : fiche de réception

À chaque livraison : n° série par disque (photo labels), versions
firmware (HBA/expander/SSD), SMART initial archivé, test burn-in
planifié, étiquetage rack/U conforme au plan. **Aucun disque ne
rentre en production sans fiche de réception complète.**

## 300. Gabarit : plan de nommage

`dc1-r07-n03` (datacenter, rangée, nœud), `osd.45`, pools
`rbd-vm-prod-ssd`, `rgw-backup-hdd-ec83`. Un nommage **prédictible**
= des scripts qui marchent et des astreintes moins douloureuses.
Documente la convention, fais-la respecter par tous (humains et
Ansible).

## 301. Ansible : l'automatisation minimale

Playbooks : inventaire disques (`lsblk` + SMART), déploiement
cephadm, collecte `storcli`, mise à jour firmware par vagues,
vérification post-reboot (chrony, OSD up, HEALTH_OK). Tout ce qui
est fait 2× à la main doit être en playbook à la 3e fois.

## 302. La revue annuelle du stockage

1×/an : AFR par modèle, croissance des données vs prévision,
TCO réel vs budget, état des garanties (renouvellement ?),
roadmap (les 245 To arrivent — faut-il attendre pour l'extension ?),
exercice de panne (game day). C'est cette revue qui transforme
l'exploitation en **pilotage**.

## 303. Les indicateurs pour la direction

- €/To utile (TCO 5 ans), par tier (chaud/froid).
- Disponibilité (9x) : mesurée, pas promise.
- Temps moyen de recovery (issu des game days).
- kWh/To/an et PUE du stockage.
- Âge moyen du parc et fin de garantie (risque capacitaire).
Parle la langue de la direction : **€, risques, délais** — pas IOPS.

## 304. Erreurs d'achat classiques (récap)

1. Acheter au €/To brut sans TCO.
2. Un seul fournisseur qualifié.
3. Baies U.2 pour du neuf dense (vs E3.S).
4. HBA en IR « parce que c'était le défaut ».
5. Réseau 25G pour du full NVMe.
6. Pas de spare (le premier RMA arrive toujours un vendredi).
7. Oublier le pic de spin-up dans l'onduleur.
8. Cache tiering Ceph (déprécié).
9. RAID 5 sur 20 To.
10. Pas de burn-in « pour gagner du temps ».

## 305. Ce que Zelef doit exiger de ses fournisseurs

Firmware IT par écrit, baies UBM/E3.S, alimentations Titanium,
garantie 5 ans avec SLA J+1, double sourcing, devis datés avec
délais fermes (allocation 2026), et **un interlocuteur technique**
qui connaît la différence entre un SFF-8654 et un SFF-8644. Si le
commercial ne la connaît pas : change d'interlocuteur.

## 306. Pour aller plus loin (dans tes guides)

- `proxmox_guide.md` : Ceph HCI, NUT, ZFS.
- `onduleurs_ups_guide.md` : dimensionnement électrique, groupes.
- `zabbix_guide.md` : supervision (templates SMART/Ceph).
- `debian_ubuntu_guide.md` : tuning noyau, multipath, chrony.

---

# PARTIE V — APPROFONDISSEMENTS

## 307. ZFS vs Ceph : le duel honnête

| Critère | ZFS | Ceph |
|---|---|---|
| Échelle | 1 serveur (jusqu'à ~1 Po) | cluster (multi-Po) |
| Redondance | RAID-Z / miroir local | réplica / EC distribués |
| Survie nœud | non (sauf réplication) | oui |
| Complexité | modérée | élevée |
| Cas roi | NAS, backup, PME | cloud, S3, VM à l'échelle |

Les deux ne s'opposent pas : ZFS **dans** le nœud (boot, cache),
Ceph **entre** les nœuds. Et Proxmox parle aux deux (voir ton
`proxmox_guide.md`).

## 308. ZFS : recordsize et volblocksize

Le réglage qui change tout : `recordsize=1M` pour du séquentiel
(médias, backup), `recordsize=16K` ou `volblocksize=16K` pour des
VM/bases. Un mauvais recordsize = amplification d'écriture ×8.
Règle : aligne sur la taille d'IO de l'application, et ne change
jamais sur un dataset plein (réécriture nécessaire).

## 309. ZFS : ARC, L2ARC, ZIL/SLOG

- **ARC** : cache lecture en RAM (50 % de la RAM par défaut — à
  ajuster : `zfs_arc_max`).
- **L2ARC** : extension sur SSD (utile si ARC saturé **et**
  workload relisible — sinon du gaspillage).
- **SLOG** : device séparé pour le ZIL (écritures synchrones :
  NFS, bases). **Le SLOG doit être le SSD le plus endurant et le
  plus faible en latence** (3 DWPD+, PLP) — c'est le block.db de ZFS.
Sans SLOG, les écritures sync sur HDD = ~100 IOPS. Avec : ~10 000.

## 310. ZFS : copies=2 et ditto blocks

`zfs set copies=2` : chaque bloc écrit 2× **sur le même pool**.
Protège contre la corruption/bitrot, pas contre la panne disque.
Utile sur du miroir critique ou du single-disk. Ne remplace pas
un vrai miroir.

## 311. mdadm : le RAID logiciel Linux

Toujours pertinent en 2026 pour : boot en RAID 1, petits volumes,
serveurs sans ZFS/Ceph. `mdadm --create /dev/md0 --level=6
--raid-devices=8`. Limites : pas de checksums (corruption silencieuse
possible), rebuild lent comme tout RAID. Pour du sérieux : ZFS.

## 312. LVM : thin provisioning et cache

LVM thin : sur-allocation (pratique en virtualisation). LVM cache :
SSD devant HDD (le « tiering » local qui marche, contrairement au
cache tiering Ceph — car local et simple). `lvmcache` en writethrough
pour la lecture, writeback avec prudence (perte du cache = perte de
données en writeback sans redondance).

## 313. Filesystems : ext4 vs XFS en 2026

| Critère | ext4 | XFS |
|---|---|---|
| Maturité | maximale | maximale |
| Gros fichiers | bien | **excellent** |
| Parallélisme | correct | meilleur |
| Réparation (fsck) | rapide | plus lent sur gros volumes |

Pour OSD Ceph : BlueStore, pas de FS. Pour le reste (boot, données
applicatives) : XFS par défaut en 2026 sur RHEL-like, ext4 ailleurs.
Les deux sont sains ; btrfs reste déconseillé en production RAID.

## 314. Tuning Linux : le scheduler

- NVMe : `none` (pas de scheduler — le SSD gère).
- SSD SATA/SAS : `none` ou `mq-deadline`.
- HDD : `mq-deadline` ou `bfq` (ce dernier si multi-tenant).
Vérifie : `cat /sys/block/sdX/queue/scheduler`. Un `cfq` oublié sur
du NVMe = latence +30 % (vécu).

## 315. Tuning Linux : nr_requests et read_ahead

- `nr_requests` : 256-1024 sur SSD/NVMe (plus = plus de parallélisme).
- `read_ahead_kb` : 128 par défaut ; 1024-4096 en séquentiel pur
  (backup), 0-128 en aléatoire (bases).
- `max_sectors_kb` : 1024+ sur HBA modernes.
Ces réglages se mettent en **udev** (persistants), pas à la main.

## 316. Tuning Linux : l'affinité IRQ

`irqbalance` suffit souvent, mais sur nœud NVMe chargé : affinité
manuelle des queues NVMe (`/proc/irq/*/smp_affinity`) sur des cœurs
dédiés, séparés des OSD. Mesure avec `mpstat -P ALL` : un cœur à
100 % softirq pendant que 31 autres dorment = affinité à revoir.

## 317. Huge pages et Ceph

Les OSD allouent beaucoup de petits buffers : les huge pages (2 Mo)
réduisent la pression TLB sur gros nœuds. À tester par workload —
le gain est réel sur 32+ OSD par nœud, marginal en dessous. Comme
tout tuning : **bench avant/après**, pas de cargo cult.

## 318. La déduplication : le calcul honnête

La dédup (ZFS, VAST) économise 2-5× sur VDI/sauvegardes, ~1,2× sur
données généralistes. Coût : **RAM** (table DDT : ~5 Go par To
dédupliqué sous ZFS, ordre de grandeur) + CPU. Règle : ne déduplique
que si le ratio mesuré en lab dépasse 2× **et** que tu as la RAM.
Sinon : compression seule.

## 319. La compression : lz4/zstd

- **lz4** : quasi gratuit en CPU, ratio ~2× sur texte/logs/VM.
- **zstd** : meilleur ratio, coût CPU modéré (niveaux 1-3 en temps réel).
Sur Ceph BlueStore : compression par pool (`compression_algorithm zstd`).
Sur ZFS : `compression=zstd-3`. **Toujours activer** sauf données déjà
compressées/chiffrées (médias, backups chiffrés : la compression ne
sert à rien et coûte du CPU — mesure).

## 320. Le chiffrement et la compression : l'ordre compte

Chiffré puis compressé = incompressible. Donc : compresse **avant**
de chiffrer (ZFS le fait dans le bon ordre en natif). Si tes backups
sont chiffrés côté client avant envoi : désactive la compression côté
stockage, c'est du CPU perdu.

## 321. La bande magnétique : LTO en 2026

LTO-9 : 18 To natifs (45 To compressés), ~400 Mo/s. Usage : archivage
froid ultime, air gap anti-ransomware. Coût : ~10 €/To le média
(ordre de grandeur, à vérifier) + librairie. Le **vrai** 3-2-1
d'un gros site inclut encore de la bande en 2026. Inconvénient :
temps d'accès (minutes), gestion de librairie = métier.

## 322. Archivage : la pyramide des tiers

```
 Chaud : NVMe réplica 3      < 1 ms    ~2000 €/To
 Tiède : TLC EC 4+2          ~2 ms     ~500 €/To
 Froid : QLC EC 8+3          ~5 ms     ~250 €/To
 Glacial : HDD EC 8+3        ~10 ms    ~150 €/To
 Air gap : LTO               minutes   ~30 €/To (média)
```
(Ordres de grandeur, à vérifier.) Le tiering = placer chaque donnée
au bon étage. 80 % des données ont plus d'un an : elles n'ont rien
à faire sur du NVMe.

## 323. RGW avancé : multisite

RGW multisite : réplication asynchrone entre zones (active-active ou
active-passive). Usage : PRA inter-sites S3. Pièges : le lag de
réplication (surveille `radosgw-admin sync status`), les conflits
de versions (politique last-write-wins par défaut), et le **coût
réseau** inter-sites (facturé au Go chez beaucoup d'opérateurs).

## 324. RGW avancé : le sharding des buckets

Un bucket avec des millions d'objets sans sharding = index qui rame.
`rgw_override_bucket_index_max_shards` et resharding en ligne :
à planifier **avant** que le bucket n'explose (le resharding d'un
bucket de 100 M d'objets prend des heures). Règle : shard dès la
création pour les buckets applicatifs à forte croissance.

## 325. CephFS avancé : les MDS en pratique

1 MDS actif + 2 standby par filesystem en production. Le MDS tient
tout son cache en RAM : **64-128 Go** par MDS actif sur gros volumes.
Symptôme d'un MDS sous-dimensionné : `mds_cache_memory_limit`
dépassé, latences `ls` catastrophiques. Sépare les filesystems par
usage (homes vs scratch) : un `find /` d'un utilisateur ne doit pas
évincer le cache d'un autre.

## 326. CephFS : les snapshots

Snapshots au niveau répertoire (`mkdir .snap/x`) : instantanés,
cohérents, pas de surcoût initial. Idéal pour les homes et les
dépôts. Limite : le MDS doit suivre les snaps (ne pas en garder
des centaines). Ce n'est toujours pas un backup (section 289).

## 327. RBD avancé : mirroring inter-sites

`rbd-mirror` : réplication asynchrone d'images entre clusters
(journal-based). RPO : secondes à minutes. Usage : PRA VM
inter-sites. À tester : le failover (promote) et le failback —
un mirroring jamais testé en bascule est un espoir, pas un PRA.

## 328. RBD : les clones et le flatten

Clone = image COW sur snapshot parent. Pratique pour déployer 100 VM
depuis un template (espace partagé). Piège : le parent ne peut pas
être supprimé tant que des clones vivent, et un parent sur un pool
lent ralentit tous les clones → **flatten** les clones devenus
« adultes » (`rbd flatten`).

## 329. Les pools : un par usage

Un pool = une politique (device class, réplica/EC, PG). Crée :
`rbd-vm-ssd` (réplica 3), `rbd-backup-hdd` (EC), `rgw-data` (EC 8+3),
`cephfs-data-froid` (EC), `cephfs-metadata` (réplica 3 SSD).
**Jamais** de pool fourre-tout : on ne change pas la politique d'un
pool plein sans migration.

## 330. Le crush map : les buckets

```
host → chassis → rack → row → room → datacenter → root
```
CRUSH place les répliques en remontant les buckets : `chooseleaf`
sur `host` = 1 réplique par host. Pour du stretch : des buckets
`datacenter`. **Dessine ton crush map avant** de créer les règles :
un bucket oublié (rack non déclaré) = placement qui ignore la
topologie réelle.

## 331. Failure domains : la hiérarchie

| Domaine | Protège contre | Coût |
|---|---|---|
| osd | panne disque | 0 (défaut logique) |
| host | panne nœud | nœuds ≥ réplica |
| rack | panne rack/switch | câblage inter-racks |
| datacenter | perte de site | 2e site |

Monte d'un cran quand le budget le permet. La plupart des PME
s'arrêtent à `host` — c'est déjà très bien avec 4+ nœuds.

## 332. Étude de cas : la panne silencieuse

Scénario vécu (type) : un SSD NVMe « lent » (pas mort) fait grimper
la latence p99 de tout le cluster pendant 3 semaines avant d'être
identifié. Le SMART était OK, mais `bluestore_slow_ops` crachait.
Leçon : surveille les **slow ops**, pas seulement les pannes franches
(section 130). Le disque a été remplacé, p99 revenue à la normale
en 1 h. Coût de la non-détection : 3 semaines de perfs dégradées.

## 333. Étude de cas : le câble

Scénario type : après un déménagement de rack, des erreurs CRC
intermittentes sur un domaine SAS. 2 semaines de diagnostic (disques
suspectés, firmware reflashé) avant de trouver : un SFF-8644 **mal
enclenché** (vis non serrées, vibrations). Leçon : après chaque
intervention physique, `storcli` + compteur d'erreurs à J+1, J+7.
Et serre les vis (section 86).

## 334. Étude de cas : l'orage

Scénario type : micro-coupures répétées, onduleur sous-dimensionné
pour le pic de spin-up (section 200). Les nœuds tombent un par un,
Ceph passe en degraded, puis les NUT arrêtent le reste. Au redémarrage :
spin-up simultané → l'onduleur disjoncte → re-belote. Leçon : **teste
le redémarrage complet après coupure**, pas seulement l'arrêt. C'est
le test que personne ne fait et qui sauve les mises en production.

## 335. Étude de cas : le firmware tueur

Scénario type (documenté industrie) : un bug firmware sur un lot de
SSD provoque des pannes corrélées (même heure, même modèle). Le
cluster survit parce que les disques étaient **mélangés par lot
d'achat** (2 fournisseurs, section 294). Leçon : le double sourcing
n'est pas du luxe, c'est de l'assurance. Et ne flashe jamais tout le
parc le même jour (section 226).

## 336. La checklist de mise en production

- [ ] Burn-in OK, fiches de réception complètes
- [ ] Firmware inventorié, mode IT vérifié
- [ ] PG/OSD dans 100-200, autoscaler on
- [ ] Réseau : MTU, LACP, VLAN séparés, 0 erreur
- [ ] Électrique : PDU A/B, NUT testé 2× (arrêt + redémarrage)
- [ ] Supervision : seuils, alertes testées
- [ ] Runbooks imprimés (pas juste dans le wiki)
- [ ] Game day n°1 planifié (kill OSD)
- [ ] Spares en stock, RMA testé une fois
- [ ] Doc d'exploitation relue par un tiers

## 337. La checklist d'audit annuel

- [ ] AFR par modèle, bannissements éventuels
- [ ] Garanties : expirations à 12 mois
- [ ] TCO réel vs prévisionnel
- [ ] Croissance vs capacité : date de saturation
- [ ] Firmware : versions à jour ?
- [ ] Game day réalisé, résultats archivés
- [ ] Clés cephx / RGW : rotation et nettoyage
- [ ] Test de restauration backup (pas juste « backup OK »)

## 338. Lexique énergie du stockage (pour ton métier)

| Terme | Sens stockage |
|---|---|
| W/To | puissance par To utile (1 = HDD froid, 30+ = NVMe) |
| PUE | multiplicateur salle (1,3-1,6 typique) |
| Spin-up | pic 2× au démarrage des HDD (10-20 s) |
| Titanium | 96 % de rendement alim (imposé au neuf) |
| kWh/To/an | l'unité du TCO énergie (~15 pour HDD, ~700 pour NVMe) |
| ΔT air | 1,7 m³/h par watt pour 2 °C (règle terrain) |

## 339. Formules à garder sous la main

```
PG            = (100 × OSD) / réplica        → puissance de 2
Brut utile    = To_utiles × (1+surcoût) × 1,15 (marge)
Disques       = Brut / To_par_disque (en Tio !)
block.db      = HDD × 2,5 % (4 % RGW)
RAM nœud      = OSD × 3-4 Go + 16 Go
CPU nœud      = OSD × 1-2 cœurs
Réseau nœud   = débit_disques × réplica × 1,2 (pire cas)
W_toit        = W_nœuds × PUE ; Onduleur ≥ 1,5 × W_toit (+ pic spin-up)
Rebuild       = To_disque / débit_effectif
€/To/an       = TCO_5ans / 5 / To_utiles
```

## 340. Les 10 commandements du stockage datacenter

1. Un OSD par disque, toujours.
2. IT mode ou rien (sous Ceph/ZFS).
3. 100-200 PG par OSD, autoscaler on.
4. Le réseau se dimensionne après les disques, jamais avant.
5. Burn-in avant production, sans exception.
6. Le RAID n'est pas un backup ; le snapshot non plus.
7. 70 % = alerte, 85 % = critique, jamais 95 %.
8. Titanium, PDU A/B, NUT testé 2×/an.
9. Deux fournisseurs qualifiés, toujours.
10. Si ce n'est pas écrit (doc, runbook), ça n'existe pas.

## 341. Tableau final : que choisir en 2026 (résumé décideur)

| Besoin | Architecture | Ordre de grandeur €/To utile |
|---|---|---|
| VM critiques, < 1 ms | Ceph NVMe réplica 3 / HCI | ~2 000 € |
| S3 / backup chaud | Ceph EC sur TLC ou QLC | ~300-500 € |
| Archivage froid | HDD EC 8+3 / LTO | ~150 € / ~30 € |
| IA training | NVMe Gen5 + 400G / WEKA | à chiffrer |
| PME simple | 3 nœuds HCI NVMe | ~300 € |

## 342. Dernier mot : la méthode Zelef

Ton métier, c'est l'énergie et les systèmes : applique la même
rigueur au stockage qu'à une note de calcul électrique. Chaque
dimensionnement de ce guide suit la même méthode : **hypothèses
écrites → formules → chiffres → marges → vérification terrain**.
Un dimensionnement sans hypothèses écrites est une opinion ; avec
des hypothèses écrites, c'est un document d'ingénierie. Et quand
un commercial te dit « ça passe », demande-lui de signer le calcul.

---

**FIN DU GUIDE — 342 sections. Vérifications web au 27/09/2026.**
**Rappel : prix en allocation tendue (sept. 2026) — devis datés exigés, à vérifier.**
**Guides liés : `proxmox_guide.md`, `onduleurs_ups_guide.md`, `zabbix_guide.md`, `debian_ubuntu_guide.md`.**

---

# PARTIE W — TUNING AVANCÉ ET SUJETS EXPERTS

## 343. msgr v2 et chiffrement du trafic cluster

Ceph Messenger v2 : protocole sécurisé (authentification + chiffrement
optionnel du trafic OSD). Active `ms_cluster_mode secure` sur les
clusters sensibles. Coût : CPU (AES-NI quasi gratuit sur CPU modernes).
Sur 100G+, mesure avant/après : le chiffrement peut coûter 5-10 %
de débit sur petits IO.

## 344. Plugins d'erasure coding : ISA-L vs Jerasure

| Plugin | Vitesse | Usage |
|---|---|---|
| `isa-l` (Intel) | rapide (SIMD) | **défaut recommandé** x86 |
| `jerasure` | portable | non-x86, compatibilité |
| `shec` | tolère plus | cas exotiques |

`ceph osd erasure-code-profile set` : choisis `isa-l` sur x86_64,
toujours. Un profil EC créé en `jerasure` par défaut sur vieux
tutos = 2-3× plus lent à l'encodage.

## 345. Crimson OSD : l'avenir (à surveiller)

Crimson = la réécriture des OSD sur Seastar (C++ asynchrone), pour
tirer parti du NVMe (moins de threads, plus de débit par cœur). État
2026 : en développement actif, pas encore le défaut. À surveiller :
quand Crimson deviendra le défaut, les règles CPU/OSD de ce guide
(section 112) changeront (moins de cœurs par OSD).

## 346. NVMe ZNS : les zones

Zoned Namespaces : le SSD expose des zones à écriture séquentielle,
l'hôte gère le placement (fini l'amplification d'écriture cachée).
Intérêt : endurance ×2-4 sur workloads adaptés, QoS prédictible.
État 2026 : support Linux OK, adoption applicative faible (il faut
des applis « zone-aware »). Pour Ceph : expérimental — **à surveiller**,
pas à déployer en 2026.

## 347. DPU / SmartNIC : l'offload

Les DPU (NVIDIA BlueField, AMD Pensando) déportent réseau, chiffrement
et parfois stockage du CPU. Usage stockage : NVMe-oF target offload,
chiffrement IPSec/TLS, isolation. En 2026 : pertinent sur les nœuds
très denses (32 NVMe) où le CPU est le goulot. Coût : prix + complexité.
À évaluer quand le CPU du nœud sature avant le réseau.

## 348. SPDK : le fast path

Storage Performance Development Kit : drivers userspace poll-mode
(zéro interruption, zéro copie). Utilisé par les targets NVMe-oF
haute performance et certains SDS. Pour l'exploitant Ceph classique :
pas d'action directe, mais c'est ce qui tourne sous les baies NVMe-oF
que tu achètes. Retiens : si un vendeur annonce « 5 M IOPS », c'est
souvent du SPDK + RDMA.

## 349. La loi de Little : le modèle mental

`IOPS × latence = profondeur_de_file`. Si tu veux 100K IOPS à 1 ms :
il faut QD = 100. Si ton application ne fait que QD=8 : max 8K IOPS
à 1 ms, **quel que soit le SSD**. 90 % des « problèmes de perfs
stockage » sont des problèmes de parallélisme applicatif, pas de
disques. Mesure le QD réel (`iostat -x`, `avgqu-sz`) avant d'acheter
du plus rapide.

## 350. Le dimensionnement par la file d'attente

Formule inverse : `latence_cible = QD_appli / IOPS_cibles`. Exemple :
appli à QD=32 qui veut 50K IOPS → latence max = 32/50000 = 0,64 ms.
Si ton Ceph fait 2 ms p99 : impossible, il faut plus de parallélisme
ou moins de latence. Ce calcul tient sur un post-it et évite des
achats inutiles.

## 351. NFS Ganesha sur CephFS

Pour servir du NFSv4 depuis CephFS : NFS-Ganesha avec backend FSAL_CEPH.
Usage : applis legacy qui ne parlent que NFS. Pièges : le Ganesha est
stateful (HA = failover actif/passif ou cluster), et les verrous NFS
sur CephFS distribué ont leurs quirks. À tester par workload.

## 352. Samba / SMB sur CephFS

De même avec Samba (`vfs_ceph`). Usage : homes Windows, bureautique.
Le duo Samba+CTDB+CephFS donne du SMB HA. En 2026 c'est mature pour
de la bureautique ; pour du très gros débit SMB, une appliance
(PowerScale) reste plus simple.

## 353. iSCSI gateway Ceph (tcmu)

`ceph-iscsi` (tcmu-runner) : expose des images RBD en iSCSI avec
multipath et HA (2+ gateways). Usage : hyperviseurs ou applis qui
ne parlent que iSCSI (VMware !). Latence ajoutée ~0,5 ms. C'est la
passerelle propre pour intégrer Ceph à un existant VMware/Hyper-V.

## 354. NVMe-oF gateway Ceph

La passerelle NVMe/TCP pour RBD (projet en maturation 2025-2026) :
le successeur logique de l'iSCSI gateway, avec la latence NVMe.
État : à qualifier sur ta version Ceph (**à vérifier** au 27/09/2026
selon release). Si tu dois connecter des initiateurs NVMe à Ceph en
2026 : pilote d'abord, production ensuite.

## 355. Le client kernel CephFS vs FUSE

| Client | Perf | Usage |
|---|---|---|
| kernel (`mount -t ceph`) | meilleure | serveurs Linux, stable |
| FUSE (`ceph-fuse`) | moindre | debug, non-root, tests |

En production : **toujours le client kernel**. Le FUSE ne sert qu'au
dépannage. Et le client kernel doit matcher à peu près la version du
cluster (pas 3 versions d'écart).

## 356. Telemetry Ceph : opt-in/out

Ceph collecte des stats anonymes (opt-in). En entreprise : décide
explicitement (souvent opt-out pour des raisons de conformité), et
documente le choix. Les données de télémétrie sont utiles à la
communauté — si ta politique le permet, l'opt-in aide tout le monde.

## 357. SELinux / AppArmor et Ceph

Sur RHEL : SELinux en enforcing avec les policies Ceph = OK en 2026
(les soucis historiques sont réglés). Ne le désactive pas « parce que
le tuto le dit » : un cluster qui ne marche qu'en permissive a un
problème de packaging, pas de SELinux. Sur Ubuntu : AppArmor, même
logique.

## 358. Les versions Ceph : Reef, Squid, Tentacle

Releases récentes (noms de céphalopodes) : Quincy → Reef → Squid →
Tentacle (2025). Règle : reste sur une version **supportée** (N et
N-1), upgrade 1×/an. Les nouveautés (Crimson, NVMe-oF gateway)
arrivent par release : lis les release notes avant chaque upgrade,
surtout les « upgrade notes » (changement de défauts).

## 359. Le dashboard Ceph : ce qu'il vaut

Le dashboard (MGR) : santé, PG, OSD, perf, RGW. Utile pour le
quotidien et les démos. Limites : pas un vrai monitoring (pas
d'alerting fin, pas d'historique long) → **toujours** doublé par
Prometheus/Grafana ou Zabbix (voir ton `zabbix_guide.md`).

## 360. Prometheus + Grafana : les métriques d'or

`ceph_osd_op_w_latency`, `ceph_osd_op_r_latency` (p99 !),
`ceph_pg_degraded`, `ceph_osd_numpg`, `ceph_cluster_total_bytes` vs
`ceph_cluster_total_used_bytes`. Dashboard : santé + latence p99 par
pool + remplissage + recovery. Alerte : latence p99 > 2× la baseline
pendant 15 min = page.

## 361. Le benchmarking continu

Un bench mensuel automatisé (même `rados bench` 5 min sur pool dédié,
même heure) crée une **baseline**. Quand la latence dérive de 20 %
sur 3 mois : enquête (SSD qui vieillissent ? fragmentation ? réseau ?).
Sans baseline, toute dérive lente est invisible jusqu'à la plainte
utilisateur.

## 362. PRA : le plan de reprise d'activité

Le stockage est au cœur du PRA : RPO/RTO par application, pas par
technologie. Tableau type :

| Application | RPO | RTO | Moyen |
|---|---|---|---|
| VM critiques | 15 min | 1 h | réplication RBD + snapshots |
| S3 backup | 24 h | 4 h | RGW multisite |
| Archivage | 7 j | 72 h | bande hors site |

**Teste le PRA 1×/an** en vraie bascule (pas en « exercice papier »).
Un PRA non testé est une fiction coûteuse.

## 363. L'exercice de bascule : le protocole

1. Annonce, freeze des changements.
2. Bascule (promote rbd-mirror / DNS RGW / mount CephFS distant).
3. Validation applicative (pas juste « le ping passe »).
4. Mesure du RTO réel vs cible.
5. **Failback** (le retour est souvent plus dur que l'aller).
6. Post-mortem sans blâme, actions correctives datées.

## 364. Post-mortem sans blâme

Après chaque incident significatif : timeline factuelle, causes
racines (les 5 pourquoi), actions (qui, quoi, quand). **Jamais** de
nom de coupable dans le document. Un post-mortem qui cherche un
coupable produit des gens qui cachent les incidents ; un post-mortem
sans blâme produit des systèmes qui s'améliorent.

## 365. L'astreinte : l'organisation

Rotation, pas de « toujours le même ». Runbooks imprimés (section 336),
accès VPN + IPMI + dashboard testés **hors incident**, escalade écrite
(quand appeler le N+1 ? le fournisseur ?). Et : une astreinte qui sonne
plus de 2×/mois = un problème de fond (seuils, spares, ou architecture),
pas de malchance.

## 366. La formation de l'équipe

Niveau 1 (tous) : SMART de base, remplacement disque, lecture `ceph -s`.
Niveau 2 (référents) : CRUSH, PG, recovery, upgrade. Niveau 3 (expert) :
tuning, debug profond, relation éditeur. **Au moins 2 personnes** par
niveau critique (le bus factor de 1, c'est une panne en attente).

## 367. Green IT : le carbone du stockage

Deux postes : carbone **opérationnel** (kWh × intensité du mix) et
carbone **embarqué** (fabrication : ~50-100 kg CO₂e par SSD, ordre de
grandeur à vérifier — un HDD c'est ~20-40 kg). Sur 5 ans, un nœud
NVMe à 800 W en France (~50 g/kWh) = ~1,75 t CO₂e opérationnel.
Le levier n°1 : **allonger la durée de vie** (6-7 ans au lieu de 5)
et **ne pas sur-dimensionner** (le To inutile est du carbone inutile).

## 368. Le reconditionné : le bilan carbone

Un HDD reconditionné évite la fabrication d'un neuf (~30 kg CO₂e).
Pour du froid non critique : le reconditionné garanti est le meilleur
rapport €/To **et** CO₂/To. Intègre-le à la politique d'achat avec des
règles claires (section 295) au lieu de le subir en « bon plan ».

## 369. La fin de vie : DEEE et effacement

Disques en fin de vie : effacement certifié (section 286) puis filière
DEEE agréée (bordereau de suivi). Les aimants des HDD et les métaux
des SSD se recyclent ; les plateaux partent en filière. **Trace** :
n° série → certificat d'effacement → BSD. Un disque « jeté » sans
traçabilité, c'est un risque juridique.

## 370. L'assurance : le risque résiduel

Même bien conçu, un stockage peut perdre des données (incendie,
ransomware, erreur humaine en chaîne). L'assurance cyber/casse couvre
le financier, pas les données. Traduction : l'assurance ne remplace
**jamais** le 3-2-1 et le PRA testé. Elle paie la facture, pas la
reconstruction.

## 371. La conformité : ce qui s'applique

Selon secteur : RGPD (droit à l'effacement → crypto-shredding via
SED, section 287), HDS/secret (hébergement agréé), bancaire (DORA),
industrie (NIS2). Le stockage est concerné par : chiffrement,
traçabilité des accès (audit log), durées de rétention, et **droit
à l'effacement effectif** (un backup immuable Object Lock de 7 ans
vs un droit à l'effacement = tension juridique à arbitrer en amont).

## 372. Le chiffrement et le droit à l'effacement

Le crypto-shredding (destruction de la clé) est la réponse technique
au droit à l'effacement sur stockage distribué : impossible d'aller
réécrire chaque réplique, mais sans la clé, c'est du bruit. D'où
l'intérêt du chiffrement **par tenant/projet** (clés distinctes) :
effacer = détruire la clé du tenant.

## 373. Le capacity planning : la méthode

1. Mesure la croissance réelle (Go/jour par pool, 6 mois d'historique).
2. Projette linéairement + marge 30 %.
3. Date de saturation = (capacité × 0,7 - utilisé) / croissance.
4. Commande à **saturation - délai d'appro** (12 semaines en 2026 !).
5. Revois trimestriellement.
Un tableau, 5 lignes, mis à jour chaque trimestre : c'est tout ce
qu'il faut. La plupart des saturations « surprises » sont des
plannings non mis à jour.

## 374. La déduplication des projets

Avant d'acheter de l'extension : chasse le gaspillage. Les classiques :
snapshots oubliés (RBD/ZFS), buckets RGW sans lifecycle, VM éteintes
avec disques alloués, 3 copies du même dataset. Un « nettoyage de
printemps » annuel récupère souvent **10-20 %** — soit un an de
croissance offert.

## 375. Les politiques de lifecycle S3

RGW supporte les règles de lifecycle (transition vers EC froid,
expiration). Configure dès la création des buckets applicatifs :
`logs/` → expiration 90 j, `backups/` → transition EC 30 j. Sans
politique, les buckets ne font que grossir — c'est mathématique.

## 376. Le monitoring des coûts (FinOps stockage)

Tag par projet/pool : Go/mois, IOPS, €. Facturation interne au To
avec les vrais coûts (TCO, pas CAPEX). Effet : les équipes nettoient
spontanément quand elles voient la facture. Le stockage « gratuit »
est toujours gaspillé.

## 377. Les erreurs humaines : la défense en profondeur

`rm`, `rbd snap purge`, mauvaise règle CRUSH : la défense =
confirmations (`--yes-i-really-mean-it`), RBAC (pas d'admin à tout
le monde), snapshots avant opérations, et **délai de grâce** (corbeille,
soft-delete RGW). On ne supprime jamais vite en production.

## 378. La documentation vivante

La doc qui n'est pas relue meurt. Rituel : à chaque incident, la
section concernée est relue et corrigée. Responsable doc nommé,
revue trimestrielle de 30 min. Une doc de 50 pages à jour vaut mieux
que 500 pages obsolètes.

## 379. La veille technologique : le rituel

30 min/mois : release notes Ceph, roadmaps NAND (Samsung/Solidigm/
KIOXIA/Micron), specs SNIA/EDSFF/CXL, Greensheet marché. C'est ce
rituel qui t'évite d'acheter de l'U.2 en 2027 ou de rater les 245 To.
Ce guide est un instantané au 27/09/2026 : **re-vérifie les prix,
les dispos et les roadmaps à chaque projet**.

## 380. Conclusion : l'esprit du guide

Le stockage datacenter 2026, c'est : du NVMe E3.S partout où la
latence compte, du HDD CMR là où le To compte, du logiciel (Ceph)
entre les deux, et de l'énergie en fil rouge. Les formules de ce
guide tiennent sur deux pages (section 339) ; tout le reste, c'est
de la discipline d'exécution : burn-in, monitoring, spares, game
days, doc. Les clusters qui survivent 5 ans sans incident majeur
ne sont pas les plus chers — ce sont les mieux opérés.

---

**FIN DU GUIDE — 380 sections numérotées, vérifications web au 27/09/2026.**
**Fichier : `~/workspace/user/files/datacenter_stockage_guide.md`**
**Rappel : prix en allocation tendue (sept. 2026) — devis datés exigés, faits marqués « à vérifier » à valider.**
**Guides liés : `proxmox_guide.md`, `onduleurs_ups_guide.md`, `zabbix_guide.md`, `debian_ubuntu_guide.md`.**

---

# PARTIE X — ANNEXES PRATIQUES

## 381. Commandes HBA : storcli (mémo)

```
storcli /c0 show                    # état contrôleur
storcli /c0 show all                # tout (long)
storcli /c0/eall/sall show          # tous les disques
storcli /c0/e0/s4 show              # détail un disque
storcli /c0/e0/s4 start locate      # LED localisation
storcli /c0/e0/s4 stop locate
storcli /c0 show rebuild            # rebuilds en cours
```
En script : parse la sortie `show all` et pousse les compteurs
d'erreurs vers Zabbix. Une erreur média qui augmente = RMA.

## 382. Commandes NVMe : nvme-cli (mémo)

```
nvme smart-log /dev/nvme0           # santé (percentage_used !)
nvme error-log /dev/nvme0           # erreurs
nvme id-ctrl /dev/nvme0             # identité, firmware
nvme format /dev/nvme0 --ses=2      # crypto erase (SED)
nvme set-feature /dev/nvme0 -f 0x0c  # états de puissance
```
`percentage_used` > 70 % = planifier le remplacement (section 180).

## 383. Commandes SMART : smartctl (mémo)

```
smartctl -a /dev/sdX                # tout
smartctl -t short /dev/sdX          # test court
smartctl -t long /dev/sdX           # test long (heures)
smartctl -l selftest /dev/sdX       # résultats
smartd                              # daemon de surveillance
```
Attributs critiques : 5, 187, 188, 197, 198 (section 166).

## 384. Commandes Ceph : le mémo quotidien

```
ceph -s ; ceph health detail        # santé
ceph osd df tree                    # OSD, PG, remplissage
ceph osd pool autoscale-status      # PG vs cible
ceph pg stat                        # états PG
ceph osd out <id>                   # sortir un OSD
ceph balancer status                # équilibrage
rados bench -p <pool> 30 write      # bench (pool dédié !)
ceph daemon osd.<id> perf dump       # perfs fines
```

## 385. Commandes réseau : le mémo

```
ethtool -S ethX | grep -i 'crc\|drop\|err'   # erreurs
ping -M do -s 8972 <ip>                      # MTU 9000 bout-en-bout
lspci -vv | grep -A2 LnkSta                  # PCIe négocié
ibstat / ibv_devices                         # RDMA si présent
```

## 386. Matrice câbles : que brancher où

| De → Vers | Câble | Débit max |
|---|---|---|
| HBA 9500-16e → JBOD 12G | SFF-8644 ↔ SFF-8644 | 4×12G par câble |
| HBA 9600-16e → JBOD 24G | SFF-8674 ↔ SFF-8674 | 4×24G par câble |
| HBA 9500-16i → backplane | SFF-8654 ↔ SFF-8654/8643* | selon brochage |
| Backplane NVMe direct | MCIO / SlimSAS x4 | Gen5 x4 par baie |

\* 8654↔8643 : uniquement avec adaptateur/câble au bon brochage
(SFF-9402) — vérifie la doc du châssis (section 218).

## 387. Matrice disques : que mettre où

| Disque | Baie OK | Usage |
|---|---|---|
| HDD SAS 12G | SAS 12G/24G | capacitaire, dual-path |
| HDD SATA | SAS/SATA | capacitaire simple |
| SSD SATA | SAS/SATA | boot, db petit cluster |
| SSD NVMe U.2 | U.2 / U.3 | existant Gen4 |
| SSD NVMe U.3 | U.3 (UBM) | mixte tri-mode |
| SSD E3.S | E3.S | **neuf 2026+** |
| SSD E1.S | E1.S | 1U dense |

## 388. Glossaire complémentaire (20 termes)

| Terme | Définition |
|---|---|
| AOC | Active Optical Cable : câble optique actif (10-100 m). |
| AWG | Jauge du câble (plus petit = plus gros = plus loin). |
| Backplane | Fond de panier : connectique des baies. |
| Blast radius | Périmètre d'impact d'une panne. |
| BBU / CacheVault | Protection du cache RAID (batterie/supercondensateur). |
| CMR | Conventional Magnetic Recording (HDD datacenter). |
| COW | Copy-On-Write (snapshots, clones). |
| DDT | Dedup Table (ZFS) : ~5 Go RAM par To dédupliqué. |
| ECN / PFC | Contrôle de congestion / de flux (RoCE lossless). |
| FDP | Flexible Data Placement (NVMe, section 211). |
| GFS | Grandfather-Father-Son (rétention backup). |
| LACP | Agrégation de liens (802.3ad). |
| MLAG | LACP multi-châssis (2 switchs). |
| OCP | Open Compute Project (specs SSD 2.x). |
| PLP | Power Loss Protection (condensateurs SSD). |
| RPO / RTO | Perte max / reprise max (PRA). |
| SED | Self-Encrypting Drive (TCG Opal). |
| SMR | Shingled Magnetic Recording (à bannir en RAID). |
| WORM | Write Once Read Many (archivage, Object Lock). |
| ZNS | Zoned Namespaces (NVMe, section 346). |

## 389. FAQ achats (1/2)

**Q : 12G ou 24G pour du HDD neuf ?**
R : 12G. Le 24G n'apporte rien à des HDD à 300 Mo/s (section 44).
**Q : U.2 ou E3.S pour du neuf ?**
R : E3.S (section 66) — sauf contrainte de châssis existant.
**Q : Combien de PG pour démarrer ?**
R : Laisse l'autoscaler faire, pools en `bulk: true` (section 110).
**Q : RAID matériel ou HBA pour Ceph ?**
R : HBA IT, toujours (section 29).

## 390. FAQ achats (2/2)

**Q : Quelle endurance pour du block.db ?**
R : 3 DWPD TLC minimum, jamais de QLC (sections 170, 222).
**Q : Faut-il du 100G pour Ceph ?**
R : Oui en full NVMe (cluster séparé), 25G suffit en HDD (section 114).
**Q : Quel onduleur pour 4 nœuds NVMe ?**
R : 10 kVA mini (section 189), avec le pic de spin-up si HDD (200).
**Q : MinIO ou Ceph RGW ?**
R : MinIO = simple/mono-site ; RGW = critique/multi-sites (150).

## 391. Tableau : qui consomme quoi (rappel énergie)

| Équipement | Conso typique | Source |
|---|---|---|
| HDD 20-30 To | 8-10 W actif | ordre de grandeur |
| NVMe Gen5 7,68 To | 17-18 W actif / 5 W idle | vérifié 27/09/2026 |
| HBA 9500-16e | 8,74 W | vérifié 27/09/2026 |
| NIC 100G | ~20 W | ordre de grandeur |
| Switch 32×100G | ~300 W | ordre de grandeur |
| Nœud 60 baies complet | ~980 W au mur | calcul section 184 |
| Nœud 12 NVMe complet | ~800 W au mur | calcul section 185 |

## 392. Tableau : prix indicatifs (rappel, à vérifier)

| Produit | Prix constaté sept. 2026 | Statut |
|---|---|---|
| HDD Exos M 30 To | ~565-600 $ (~19 $/To) | vérifié |
| NVMe entreprise Gen5 | ~350-600 €/To | ordre de grandeur |
| HBA 9500-16i/e | ~250-450 € | ordre de grandeur |
| SSD QLC 30,72 To | ~250 €/To | ordre de grandeur |
| Switch 32×100G | ~15-30 k€ | ordre de grandeur |

Marché en allocation tendue : **devis datés exigés** (section 213).

## 393. Index des schémas ASCII du guide

- Section 3 : paysage DAS/NAS/SAN/SDS
- Section 40 : arbre de décision HBA
- Section 75 : HBA → JBOD simple
- Section 76 : cascade (daisy chain)
- Section 77 : double domaine redondant
- Section 104 : matrice RAID/EC/JBOD
- Section 105 : architecture Ceph
- Section 276 : topologie réseau Ceph
- Section 322 : pyramide des tiers

## 394. Index des tableaux de dimensionnement

- Sections 115-119 : 100 To utiles NVMe (disques, nœuds, réseau, RAM/CPU)
- Section 120 : BOM nœud NVMe chiffrée
- Section 140 : 500 To hybrides chiffrés
- Section 147 : prix hybride
- Sections 184-185 : bilans de puissance
- Sections 237-242 : 4 cas (1 Po backup, IA, PME, 2 Po S3)
- Section 243 : comparatif €/To/an
- Section 292 : gabarit TCO

## 395. Anti-sèche : les 12 chiffres à connaître par cœur

1. 100-200 PG/OSD (Ceph)
2. 1 OSD = 1 disque
3. 2,5 % db (4 % RGW)
4. ≤ 15 HDD par NVMe de db
5. 22,5 Gb/s effectifs par voie 24G
6. 1024 SAS / 32 NVMe par HBA
7. 70 % alerte / 85 % critique (remplissage)
8. 5 ans de garantie = durée d'amortissement
9. 96 % = rendement Titanium
10. 1,7 m³/h par watt (ΔT 2 °C)
11. 19 $/To = HDD 30 To (sept. 2026)
12. 3-2-1 = la règle des backups

## 396. Ce que ce guide ne couvre pas (pistes)

- Le tuning CephFS très large échelle (> 1 Po de metadata),
- Le détail des firmwares par constructeur (errata),
- La tarification cloud (S3 managé vs self-hosted),
- Le stockage Kubernetes (Rook, Longhorn, OpenEBS) — piste de guide,
- La conformité sectorielle détaillée (HDS, DORA),
- Le dimensionnement des salles (cf. guides onduleurs/Proxmox).

## 397. Historique des vérifications

| Date | Objet |
|---|---|
| 27/09/2026 | SAS 24G (Broadcom 9600/9670, fiches revendeurs) |
| 27/09/2026 | HBA 9500 (specs, conso 6,12/8,74 W) |
| 27/09/2026 | SSD : D7-PS1010, CD9P-R, CD8P, D5-P5430, PM9D3 |
| 27/09/2026 | EDSFF E1.S/E3.S, Micron 9650 Gen6, KIOXIA NX1 |
| 27/09/2026 | CXL 2.0/3.x, Marvell Structera, Samsung/SK/Micron |
| 27/09/2026 | PLC (non trouvé en produit), SSD 245 To (Samsung BM1773) |
| 27/09/2026 | HDD Exos M 30 To (~19 $/To), marché en allocation |
| 27/09/2026 | Ceph PG (docs), block.db sizing, alternatives SDS |
| 27/09/2026 | Supermicro SSG-6049P-E1CR60H, gamme EDSFF |

## 398. Comment maintenir ce guide à jour

À chaque projet : re-vérifie les sections 392 (prix), 207 (roadmaps
SSD), 213 (marché), 358 (versions Ceph). Le fond (formules, topologies,
runbooks) vieillit lentement ; les références produits vite. Note la
date de chaque re-vérification dans le tableau 397. Un guide technique
non daté est un piège ; celui-ci est daté : **27/09/2026**.

---

**FIN DU GUIDE — 398 sections numérotées.**
**Fichier : `~/workspace/user/files/datacenter_stockage_guide.md`**
**Vérifications web au 27/09/2026 — faits marqués « vérifié », « à vérifier » ou « non trouvé ».**
**Guides liés : `proxmox_guide.md`, `onduleurs_ups_guide.md`, `zabbix_guide.md`, `debian_ubuntu_guide.md`.**

---

# PARTIE Y — FICHES RÉFLEXES (IMPRIMABLES)

## 399. Fiche réflexe : câblage SAS

```
12G : SFF-8643 (interne) / SFF-8644 (externe) — 4 voies = 4,8 Go/s
24G : SFF-8654 (interne) / SFF-8674 (externe) — 4 voies = 9,6 Go/s
Débits négociés : 22,5 / 12 / 6 Gb/s par voie
Règles : 2 câbles/domaine mini, étiquettes A1/A2/B1/B2,
         vis serrées, spare de chaque type dans la baie,
         jamais de cascade triple, multipath si dual-path.
Diagnostic : storcli → vitesse négociée par phy.
```

## 400. Fiche réflexe : Ceph au quotidien

```
ceph -s ; ceph health detail          # santé (chaque matin)
ceph osd df tree                      # remplissage par OSD
Alerte : 70 % warn / 85 % crit / PG degraded = immédiat
PG/OSD cible : 100-200 (autoscaler on, pools bulk:true)
Runbook panne disque : out → HEALTH_OK → purge → remplace → recrée
Ne jamais : purger avant out, toucher aux PG en upgrade,
            redémarrer tous les MON « pour voir ».
```

## 401. Fiche réflexe : énergie

```
Nœud 60 baies HDD : ~980 W au mur → ~1 470 W à climatiser (PUE 1,5)
Nœud 12 NVMe Gen5 : ~800 W au mur
Règles : Titanium imposé, PDU A/B à 100 % chacune,
         pic spin-up = 2× pendant 10-20 s (HDD),
         NUT : arrêt séquentiel testé 2×/an (arrêt ET redémarrage),
         sondes disques → supervision (45 °C warn / 50 °C crit HDD).
TCO : kWh × PUE × prix × 5 ans — toujours, pas CAPEX seul.
```

## 402. Fiche réflexe : achats

```
Checklist bon de commande :
[ ] HBA en IT (écrit), baies UBM/E3.S, alim Titanium
[ ] Disques CMR (pas SMR), 5 ans de garantie, PLP (SSD)
[ ] 2 fournisseurs qualifiés par famille
[ ] Devis daté, délai ferme, pénalités (allocation 2026)
[ ] Spares : max(2, 5 %) par modèle + 1 HBA + câbles
[ ] Burn-in 48-72 h prévu au planning (non compressible)
Neuf 2026 : E3.S Gen5 (perf) / Exos 30 To (froid) / 9500 tri-mode.
```

## 403. Fiche réflexe : pannes (top 5)

```
1. Tous les disques d'un domaine disparaissent → HBA/câble/expander
   (lspci, storcli, domaine B en attendant)
2. Latence p99 qui grimpe sans panne → SSD lent (slow ops), réseau
   (CRC), ou horloge (chrony) — dans cet ordre
3. HEALTH_WARN Ceph → ceph health detail, tout lire, ne pas rebooter
   en masse
4. Rebuild RAID qui échoue → URE (RAID 5 sur gros disques = à bannir)
5. Après coupure : ne pas redémarrer tous les nœuds d'un coup
   (pic spin-up → onduleur qui disjoncte) — séquentiel !
```

## 404. Note de version du guide

```
Version : 1.0 — 27/09/2026
Sections : 404 numérotées (398 + 6 fiches réflexes)
Vérifications : web, 27/09/2026 (tableau section 397)
Prochaine re-vérification conseillée : prix/dispos (trimestrielle),
roadmaps SSD et versions Ceph (semestrielle).
Auteur : Leo pour Zelef — chef de service systèmes & énergies.
```

## 405. Pour aller plus loin : la feuille de route 6 mois

```
Mois 1-2 : dimensionnement (formules section 339) + 2 devis par famille
           + qualification (fiches 397)
Mois 3   : commande (délais 4-12 sem. en allocation 2026) + préparation
           salle (élec, clim, PDU A/B, NUT)
Mois 4   : réception + burn-in 48-72 h + câblage (photos, étiquettes)
Mois 5   : bench + game day n°1 (kill OSD) + runbooks + supervision
Mois 6   : production progressive + revue (TCO réel, AFR, saturation)
En continu : capacity planning trimestriel, game day trimestriel,
             revue annuelle (section 302), veille mensuelle (379).
Règle d'or : on ne compresse jamais le burn-in ni les tests —
ce sont les semaines les plus rentables du projet.
```
